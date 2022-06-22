---
type: note
---

#ruby/rails

---
2022-05-18  16:51

# rails 猫でもわかるHotwire入門 Turbo編 チュートリアル2

チュートリアル1 のアプリを SPA 化するとのこと。

- ページネーション
- ソート
- 検索
- 編集
- 登録
- 削除


## Turbo Drive を有効化


Turbo Drive は `<body>` だけを置換するようになるらしい。

app/javascript/application.js

削除するだけ。

```js
-	Turbo.session.drive = false
```

## ページネーションの Turbo Frames 化
Turbo Frames は Turbo Drive の部分置換版とのこと。
`<turbo-frame>...</turbo-frame>` で囲った箇所だけ置換する。

`turbo_frame_tag` は turbo-rails の Helper
引数が id名 になる

app/views/cats/index.html.erb

```html
<h4 class="fw-bold">
  <span class="me-1">🐱</span>
  <span>ねこ</span>
</h4>

<div class="card shadow mt-3">
  <div class="card-header">
    <%= icon_with_text("search", "検索条件") %>
  </div>

  <div class="card-body">
    <%= search_form_for @search do |f| %>
      <div class="row g3 mb-3">
        <div class="col-4 col-xl-2">
          <%# `カラム名_cont` とすることで、カラムに対して LIKE を使った検索ができる%>
          <%= f.label :name_cont, "名前", class: "form-label" %>
          <%= f.search_field :name_cont, class: "form-control" %>
        </div>
        <div class="col-4 col-xl-2">
          <%# `カラム名_eq` とすることで、カラムに対して完全一致検索ができる%>
          <%= f.label :age_eq, "年齢", class: "form-label" %>
          <%= f.search_field :age_eq, class: "form-control" %>
        </div>
        <div class="col-4 d-flex align-items-end">
          <%= button_tag(icon_with_text("search", "検索"), class: "btn btn-primary me-1") %>
          <%# 検索結果検索フォームをクリアする %>
          <%= link_to "クリア", cats_path, class: "btn btn-outline-secondary" %>
        </div>
      </div>
    <% end %>
  </div>
</div>

<!-- 一覧 -->
<div class="card shadow mt-3">
  <div class="card-header">
    <%= icon_with_text("table", "一覧") %>
  </div>

  <div class="card-body mx-3">

		<%# ここで挟んだだけ %>
    <%= turbo_frame_tag "cats-list" do %>
      <div class="row py-2">
        <div class="col-4 mt-auto">
          <%= sort_link(@search, :name) %>
        </div>
        <div class="col-4 mt-auto">
          <%= sort_link(@search, :age) %>
        </div>
        <div class="col-4 d-flex justify-content-end">
          <%= link_to icon_with_text("plus-circle", "登録"),
              new_cat_path, class: "btn btn-outline-primary" %>
        </div>
      </div>

      <%= render @cats%>

      <div class="d-flex justify-content-end mt-3">
        <%= paginate @cats %>
      </div>
    <% end %>

  </div>
</div>

```

ページネーションで遷移されなくなって速くなった。

## Sort の Turbo Frames 化

すでになってる。
`<turbo-frame>` の中にあるので、Turbo Frame リクエストとして処理されている。

## 検索の Turbo Frames 化
`<turbo-frame>` の内部からのリンクではないので、指定が必要。

app/views/cats/index.html.erb

```html
    <%= search_form_for @search, html: { data: { turbo_frame: "cats-list" } } do |f| %>
      <div class="row g3 mb-3">
        <div class="col-4 col-xl-2">

```

id="cats-list" だけを置換するらしい。


## 編集の Turbo Frames 化

### 編集リンククリック時の処理

turbo_frame_tag cat とすると、dom_id を利用して "cat_1" みたいな id にしてくれるとのこと。
更新は一意な id が必要ゆえ。

```html
<%= turbo_frame_tag cat %>
と
<%= turbo_frame_tag dom_id(cat) %>
と
<%= turbo_frame_tag "cat_#{cat.id}" %>
は一緒

<turbo-frame id="cat_1">...</turbo_frame> になる。
```

`app/views/cats/_cat.html.erb`

```html
<%# 全体を turbo_frame_tag で囲う %>
<%= turbo_frame_tag cat do%>
  <div class="row py-2 border-top" >
    <div class="col-4 my-auto">
      <%= cat.name %>
    </div>
    <div class="col-4 my-auto">
      <%= cat.age %>
    </div>
    <div class="col-4">
      <div class="d-flex justify-content-end">
        <%= link_to "編集", edit_cat_path(cat), class: "btn btn-sm btn-outline-primary me-2" %>
        <%= link_to "削除", cat, method: :delete, class: "btn btn-sm btn-outline-danger" %>
      </div>
    </div>
  </div>
<% end %>
```


`app/views/cats/_form.html.erb`

```html
<%# 全体を turbo_frame_tag で覆う %>
<%# 編集リンクをクリツクすると、_cat.html.erb の <turbo-frame>部分がここに置換される %>
<%= turbo_frame_tag cat do%>
  <%= bootstrap_form_with(model: cat) do |form| %>
    <div class="row py-2 border-top">
      <div class="col-4">
        <%= form.text_field :name,
                            skip_label: true,
                            label_as_placeholder: true,
                            wrapper: false,
                            control_class: "form-control form-control-sm"
                            %>
      </div>
      <div class="col-4">
        <%= form.text_field :age,
                            skip_label: true,
                            label_as_placeholder: true,
                            wrapper: false,
                            control_class: "form-control form-control-sm"
                            %>
      </div>
      <div class="col-4">
        <div class="d-flex justify-content-end">
          <%= form.primary class: "btn btn-primary btn-sm me-2" %>
        </div>
      </div>
    </div>
  <% end %>
<% end %>
```

![[Pasted image 20220518174434.png]]

編集でここに_formが置換された!!
なんの js もコードで書いてない。



### 更新バリデーションエラー時の処理

Turbo でバリデーションエラーの HTML レスポンスは 422 : unprocessable_entity 。
Rails7 は scaffold で 422 を返すコードを生成してくれるので何もしないとのこと。

### 更新成功時の処理

```html
<p style="color: green"><%= notice %></p>

<%= render @cat %>

<div>
  <%= link_to "Edit this cat", edit_cat_path(@cat) %> |
  <%= link_to "Back to cats", cats_path %>
  <%= button_to "Destroy this cat", @cat, method: :delete %>
</div>
```

scaffold が作った元に戻した。

### キャンセル時の処理

`app/views/cats/_cat.html.erb`

```html
      <div class="col-4">
        <div class="d-flex justify-content-end">
          <%= form.primary class: "btn btn-primary btn-sm me-2" %>
          <%= link_to "キャンセル", cat, class:"btn btn-sm btn-outline-secondary" %>
        </div>
      </div>
```

![[Pasted image 20220518180838.png]]

すげー!!
Ajax でパーシャルで置換してたのが、タグの追加だけで同じ動きになってしまった。



## 編集の Turbo Streams 化

Flash 表示をするようにする。

Turbo Streams を使って複数箇所の同時更新をするようにすればいいとのこと。


app/controllers/cats_controller.rb

リダイレクトを削除する。
```ruby
  def update
    if @cat.update(cat_params)
      # リダイレクトがないと暗黙的に render が実行される
    else

```

render するフォーマットは turbo_stream とのこと。
update.html.erb ではない。

app/views/cats/update.turbo_stream.erb

```html
<%= turbo_stream.replace @cat %>
```

turbo_stream.replace ヘルパーで、パーシャルを render するらしい。

### Flash の Turbo Streams 化

app/views/layouts/application.html.erb

```html
          <div class="col my-4">
            <div id="flash">
              <%= render "flash" %>
            </div>
            <%= yield %>
          </div>

```

app/controllers/cats_controller.rb

```ruby
def update
    if @cat.update(cat_params)
      # リダイレクトがないと暗黙的に render が実行される
      flash.now.notice = "ねこを更新しました。"
    else
      render :edit, status: :unprocessable_entity
    end
  
```


app/views/cats/update.turbo_stream.erb

```html
<%= turbo_stream.replace @cat %>
<%= turbo_stream.update "flash", partial: "flash" %>
```

内部でパーシャルを render している。

これをビューヘルパーにする。

app/helpers/application_helper.rb

```ruby
  def turbo_stream_flash
    turbo_stream.update "flash", partial: "flash"
  end
```

app/views/cats/update.turbo_stream.erb

```html
<%= turbo_stream.replace @cat %>
<%= turbo_stream_flash %>
```

### 登録の Turbo Streams 化

登録も遷移せずに出来るようにする。


app/views/cats/index.html.erb

```html
        <div class="col-4 d-flex justify-content-end">
          <%= link_to icon_with_text("plus-circle", "登録"),
              new_cat_path,
              class: "btn btn-outline-primary",
              data: { turbo_frame: dom_id(Cat.new) } %>
        </div>
      </div>

      <%# 登録リンククリック時に、cats#new のフォーム部分をここに置換する %>
      <%= turbo_frame_tag Cat.new %>
      <%# 先頭に加えたい %>
      <div id="cats">
        <%= render @cats%>
      </div>

```

new.html.erb (`_form.html.erb`) を取得して Cat 一覧の一番上に表示させたい。
登録リンクに data-turbo-frame を設定して、一覧の上に turbo_frame_tag を置く。

dom_id(Cat.new) は "new_cat" になる。
turbo_frame_tag では dom_id を利用してくれるので、中身だけでいいとのこと。

登録成功したら、Cat 一覧の先頭に追加したい。
Turbo Stream の target として id="cat" を設置する。

app/controllers/cats_controller.rb

```ruby
    if @cat.save
      flash.now.notice = "ねこを登録しました。"
    else
      render :new, status: :unprocessable_entity
    end
```

これで、成功時には crdate.turbo_stream.erb が暗黙のうちに実行される。

app/views/cats/create.turbo_stream.erb

```html
<%# `#csts` の先頭に登録された cat を追加する %>
<%= turbo_stream.prepend "cats", @cat %>

<%# 登録の入力フォームの中身を空にする %>
<%= turbo_stream.update Cat.new, "" %>

<%# Flash を表示 %>
<%= turbo_stream_flash %>
```

### 削除の Turbo Streams 化

削除も遷移なしにする。

app/controllers/cats_controller.rb

```ruby
  def destroy
    @cat.destroy
    flash.now.notice = "ねこを削除しました。"
  end
```

app/views/cats/destroy.turbo_stream.erb

```html
<%# Cat詳細(`#cat_1` となる要素) を削除する %>
<%= turbo_stream.remove @cat %>

<%= turbo_stream_flash %>
```

data-confirm 属性は rails 7 では rails-ujs が廃止されたので、使えないとのこと。

rails-ujs は Turbo が引き受ける。

button_to は`<form>` を作るので、そのform に data-turbo-confirm 属性を設定する。

`app/views/cats/_cat.html.erb`

```html
        <%= link_to "編集", edit_cat_path(cat), class: "btn btn-sm btn-outline-primary me-2" %>
        <%= button_to "削除", cat, method: :delete, class: "btn btn-sm btn-outline-danger",
                              form: { data: { turbo_confirm: "本当に削除しますか?" } } %>
```

button 出なく link の場合。

```html
<%= link_to "削除", cat, class: "btn btn-sm btn-outline-danger", data: { turbo_method: :delete, turbo_confirm: "本当に削除しますか？" } %>
```

jsコードを一つも書いてないのに管理画面の一連の機能が no 遷移!!
素晴らしい。

![[Pasted image 20220519162854.png]]






