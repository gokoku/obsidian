#ruby/rails 



# Flexible Form Prototype

Rails のフォームは ActiveRecord で DB にガチで結びついていて固くて面倒くさくて、なんか「らしくない」と思ってたところ、Form Object なるものに

## Form Object デザインパターン

Rails の Model は ActiveRecord で DB をラップしている。
実際のフォームでは細かくカラムが絡んでくるので Fat Controller とか Fat Model の症状になりやすい。
でも、大雑把に見ると同じようなパターンの繰り返しに見えるのでそこはスッキリまとめられるはず。

Rails のデザインパターンとして Form Object が推奨されている背景はだいたいこんなところだろうか。

### ザクッと勝手にイメージした図

Model には ActiveRecord だけだったのが、新たに ActiveModel が加わった。
DB に変更を施さなくても柔軟にフォームを対応出来るようにする感じ。





これはフロントエンドフレームワークの形に近くなったようにも見える気がする。

[Active Model の基礎 - Railsガイド](https://railsguides.jp/active_model_basics.html)

## 実装の流れ

Book というフォームで本のタイトルと著者の CRUD を作ることにする。

面倒くさいのでスキャフォールドをもとにして Form Object を挟み込む作戦。

```shell
$ rails new forming
$ cd forming
$ rails g scaffold Form data:text
$ rails db:migrate
```



この段階では data の CRUD しかない。
ここに Form Object を挟んでフォーム項目を対応させる。

フォーム項目の永続化はこの項目を JSON にして data に格納することで実現させる。



### 実装

#### app/models/form/book.rb

Form Objectの実装をする。attributes で Serialization の形を設定する。

少しメタプログラミング

```ruby
class Form::Book
  include ActiveModel::Model
  include ActiveModel::Serialization

  REGISTABLE_ATTRIBUTES = {
    'title' => nil,
    'author' => nil
  }

  attr_accessor *REGISTABLE_ATTRIBUTES.symbolize_keys.keys

  validates :title, presence: true
  validates :author, presence: true

  def attributes
    REGISTABLE_ATTRIBUTES
  end

  def initialize(params = {})
    REGISTABLE_ATTRIBUTES.keys.each do |attr|
      self.send("#{attr}=", params[attr])
    end
  end

  def save
    return false if invalid?

    json = JSON.generate self.serializable_hash
    form = Form.new( data: json )
    form.save!
    true
  end

  def update( book_obj )
    return false if invalid?

    json = JSON.generate self.serializable_hash
    form = Form.find( book_obj.to_param )
    form.update!( data: json )
  end
end
```

#### app/controllers/forms_controller.rb

モデルオブジェクトを使い分ける。

```ruby
class FormsController < ApplicationController
  before_action :set_form, only: [:show, :edit, :update, :destroy]

  # GET /forms
  # GET /forms.json
  def index
    @forms = Form.all
  end

  # GET /forms/1
  # GET /forms/1.json
  def show
    params = JSON.parse(@form.data)
    @form_book = Form::Book.new(params)
  end

  # GET /forms/new
  def new
    @form_book = Form::Book.new
  end

  # GET /forms/1/edit
  def edit
    params = JSON.parse(@form.data)
    @form_book = Form::Book.new(params)
  end

  # POST /forms
  # POST /forms.json
  def create
    @form_book = Form::Book.new(form_book_params)

    if @form_book.save
      redirect_to forms_path, notice: 'Form was successfully created.'
    else
      render :new
    end
  end

  # PATCH/PUT /forms/1
  # PATCH/PUT /forms/1.json
  def update
    @form_book = Form::Book.new(form_book_params)

    if @form_book.update(@form)
      redirect_to forms_url, notice: 'Form was successfully updated.'
    else
      render :edit
    end
  end

  # DELETE /forms/1
  # DELETE /forms/1.json
  def destroy
    @form.destroy
    respond_to do |format|
      format.html { redirect_to forms_url, notice: 'Form was successfully destroyed.' }
      format.json { head :no_content }
    end
  end

  private
    # Use callbacks to share common setup or constraints between actions.
    def set_form
      @form = Form.find(params[:id])
    end

    # Only allow a list of trusted parameters through.
    def form_params
      params.require(:form).permit(:data)
    end

    def form_book_params
      params.require(:form_book).permit((Form==Book==REGISTABLE_ATTRIBUTES.symbolize_keys.keys)
    end
end
```

#### app/views/forms/index.html.erb

Form Object を表示するようにする。

```ruby
<p id="notice"><%= notice %></p>

<h1>Forms</h1>

<table>
  <thead>
    <tr>
      <th>id</th>
      <th>title</th>
      <th>author</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @forms.each do |form| %>
      <% form_book = Form::Book.new(JSON.parse(form.data)) %>
      <tr>
        <td><%= form.id %></td>
        <td><%= form_book.title %></td>
        <td><%= form_book.author %></td>
        <td><%= link_to 'Show', form %></td>
        <td><%= link_to 'Edit', edit_form_path(form) %></td>
        <td><%= link_to 'Destroy', form, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to 'New Form', new_form_path %>
```

#### app/views/forms/show.html.erb

Form Object を表示するようにする。

```ruby
<p id="notice"><%= notice %></p>

<p>
  <strong>title:</strong>
  <%= @form_book.title %>
</p>

<p>
  <strong>author:</strong>
  <%= @form_book.author %>
</p>

<%= link_to 'Edit', edit_form_path(@form) %> |
<%= link_to 'Back', forms_path %>
```

#### app/views/forms/\_form.html.erb

scaffold のルーチングに合わせて CRUD になるように url パスとメソッドで Form Object を操作出来るようにした。

```ruby
<%= form_for( form_book, url: url, html: {method: method} ) do |form| %>
  <% if form_book.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(form_book.errors.count, "error") %> prohibited this form from being saved:</h2>

      <ul>
        <% form_book.errors.full_messages.each do |message| %>
          <li><%= message %></li>
        <% end %>
      </ul>
    </div>
  <% end %>

  <div class="field">
    <%= form.label :title %>
    <%= form.text_field :title %>
  </div>

  <div class="field">
    <%= form.label :author %>
    <%= form.text_field :author %>
  </div>

  <div class="actions">
    <%= form.submit 'Send' %>
  </div>
<% end %>

```

#### app/views/forms/new.html.erb

```ruby
<h1>New Form</h1>

<%= render 'form', form_book: @form_book, url: forms_path, method: "POST" %>

<%= link_to 'Back', forms_path %>
```

#### app/views/form/edit.html.erb

```ruby
<h1>Editing Form</h1>

<%= render 'form', form_book: @form_book, url: form_path, method: "PUT" %>

<%= link_to 'Show', @form %> |
<%= link_to 'Back', forms_path %>
```

