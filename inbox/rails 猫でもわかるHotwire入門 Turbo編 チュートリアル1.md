---
type: note
---

#ruby/rails 

---
2022-05-18  16:48

# rails 猫でもわかるHotwire入門 Turbo編 チュートリアル1


## チュートリアル1

```shell
$ rails new cat-hotwire --css=bootstrap --skip-jbuilder --skip-action-mailbox --skip-action-mailer --skip-test --skip-active-storage --skip-action-text
```

### ロケール
config/application.rb
```ruby
config.time_zone = "Tokyo"
config.i18n.default_locale = :ja
```

[[rails 日本語ロケール]]

### Turbo Drive 無効化
app/javascript/application.js

```js
Turbo.session.drive = false
```

### scaffold

```shell
$ rails db:create
$ rails g scaffold Cat name:string age:integer
$ rails g scaffold Dog name:string age:integer
$ rails g scaffold Chick name:string age:integer
$ rails g scaffold Hedgehog name:string age:integer
$ rails g scaffold Owl name:string age:integer

$ rails db:migrate

$ bin/dev
```

この bin/dev が jsbundling-rails でのサーバ起動。

### importmap-rails と jsbundling-rails

`$ rails server` ではなく`$ bin/dev`を使う。

Rails7 では webpacker が廃止となって、importmap-rails という gem を使うやり方がデフォ。

##### importmap-rails
webpacker や esbuild を使わない方法。

<div class="rich-link-card-container"><a class="rich-link-card" href="https://zenn.dev/takeyuweb/articles/996adfac0d58fb" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://res.cloudinary.com/zenn/image/upload/s--WEMOTn_G--/co_rgb:222%2Cg_south_west%2Cl_text:notosansjp-medium.otf_37_bold:takeyuweb%2Cx_203%2Cy_98/c_fit%2Cco_rgb:222%2Cg_north_west%2Cl_text:notosansjp-medium.otf_65_bold:Rails%25207.0%2520%25E3%2581%25A7%25E6%25A8%2599%25E6%25BA%2596%25E3%2581%25AB%25E3%2581%25AA%25E3%2581%25A3%25E3%2581%259F%2520importmap-rails%2520%25E3%2581%25A8%25E3%2581%25AF%25E4%25BD%2595%25E3%2581%25AA%25E3%2581%25AE%25E3%2581%258B%25EF%25BC%259F%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2ljeVN4dTIydDRHcy1rVlRMVFh3MHZLX2w1TWxpNngxQTB1TUR3VXc9czk2LWM=%2Cr_max%2Cw_90%2Cx_87%2Cy_72/v1627274783/default/og-base_z4sxah.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Rails 7.0 で標準になった importmap-rails とは何なのか？</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://zenn.dev/takeyuweb/articles/996adfac0d58fb
		</p>
	</div>
</a></div>

webpack がファイルをまとめて import して解決してたのを、
- HTML でパッケージ名:URL を列挙してブラウザに伝える
- JS で import


Rails では、config/importmap.rb に記述して、
`<%= javascript_importmap_tags %>`　Helper を使うらしい。

##### jsbundling-rails

```shell
$ rails new --css bootstrap
```
 
この指定で jsbundling-rails を使うことになるらしい。
これは、
webpack ではなく、esbuild バンドラーを使う。
それで、server 起動の時に foreman で css ビルドも一緒に走らせる。


### seeds

db/seeds.rb

[[rails Cat チュートリアルの猫 seeds]]

```shell
$ rails db:seed
```


### ページネーション

Gemfile

```gem
gem "kaminari"
```

```shell
$ bundle install
```

app/controllers/cats_controller.rb

```ruby
def index
  @cats = Cat.page(params[:page])
end
```

app/views/cats/index.html.erb

```html

  <% end %>
</div>
<%= pagenate @cats %>
<%= link_to "New cat", new_cat_path %>
```

config/locales/kaminari.ja.yml

```yaml
ja:
  helpers:
    page_entries_info:
      more_pages:
        display_entries: "<b>%{total}</b>中の%{entry_name}を表示しています <b>%{first} - %{last}</b>"
      one_page:
        display_entries:
          one: "<b>%{count}</b>レコード表示中です %{entry_name}"
          other: "<b>%{count}</b>レコード表示中です %{entry_name}"
          zero: "レコードが見つかりませんでした %{entry_name}"
  views:
    pagination:
      first: "&laquo; 最初"
      last: "最後 &raquo;"
      next: "次 &rsaquo;"
      previous: "&lsaquo; 前"
      truncate: "&hellip;"
```

```shell
$ rails g kaminari:config
```

config/initialivers/kaminari_config.rb

```ruby
  config.default_per_page = 1o
```

1ページ 10件表示。


### 検索

Gemfile

```ruby
gem "ransack"
```

```shell
$ bundle install
```


app/controllers/cats_controller.rb

```shell
  def index
    #@cats = Cat.all
    #@cats = Cat.page(params[:page])

    @search = Cat.ransack(params[:q])
    @search.sorts = 'id desc' if @search.sorts.empty?
    @cats = @search.result.page(params[:page])
  end
```

app/views/cats/index.html.erb

```html
<%= search_form_for @search do |f| %>
  <%# `カラム名_cont` とすることで、カラムに対して LIKE を使った検索ができる%>
  <%= f.label :name_cont, "名前" %>
  <%= f.search_field :name_cont %>

  <%# `カラム名_eq` とすることで、カラムに対して完全一致検索ができる%>
  <%= f.label :age_eq, "年齢" %>
  <%= f.search_field :age_eq %>
  <%= f.submit %>

  <%# 検索結果検索フォームをクリアする %>
  <%= link_to "クリア", cats_path %>
<% end %>
```

### ソート

app/views/cats/index.html.erb

```ruby
<%= sort_link(@search, :name) %>
<%= sort_link(@search, :age) %>

<div id="cats">
```

### Bootstrap5

app/controllers/cats_controller.rb

```ruby
      redirect_to @cat, notice: "ねこを登録しました。"

			redirect_to @cat, notice: "ねこを更新しました。"

	    redirect_to cats_url, notice: "ねこを削除しました。"
```

app/helpers/application_helper.rb

```ruby
module ApplicationHelper
  def sidebar_link_to(path, emoji, text)
    classes = %w[my-1 nav-link text-white]
    classes << "active" if current_page?(path)

    link_to(path, class: classes) do
      tag.span(class: "me-2") { emoji } + tag.span { text }
    end
  end

  def icon(icon_name)
    tag.i(class: ["bi", "bi-#{icon_name}"])
  end

  def icon_with_text(icon_name, text)
    tag.span(icon(icon_name), class: "me-2") + tag.span(text)
  end
end
```


`app/views/application/_flash.html.erb`

```html
<% if notice %>
  <div class="alert alert-primary alert-dismissible">
    <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
    <%= notice %>
  </div>
<% end %>

<% if alert %>
  <div class="alert alert-danger alert-dismissible">
    <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
    <%= alert %>
  </div>
<% end %>
```


`app/views/application/_sidebar.html.erb`

```ruby
<div class="d-flex flex-column flex-shrind-0 py-3 px-3 bg-dark text-white vh-100">
  <div class="d-flex align-items-center justify-content-center py-2">
    <%= link_to "AMS", root_path, class: "text-white display-6 text-decoration-name" %>
  </div>
  <hr />

  <ul class="nav nav-pills flex-column mb-auto">
    <li class="nav-item">
      <%= sidebar_link_to(cats_path, "🐱", "ねこ") %>
    </li>
    <li class="nav-item">
      <%= sidebar_link_to(cats_path, "🐶", "いぬ") %>
    </li>
    <li class="nav-item">
      <%= sidebar_link_to(cats_path, "🐣", "ひよこ") %>
    </li>
    <li class="nav-item">
      <%= sidebar_link_to(cats_path, "🦔", "ハリネズミ") %>
    </li>
    <li class="nav-item">
      <%= sidebar_link_to(cats_path, "🦉", "フクロウ") %>
    </li>
  </ul>
</div>
```

`app/views/cats/_cat.html.erb`

```html
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
```

app/views/cats/edit.html.erb

```html
<h1>ねこを編集する</h1>

<%= render "form", cat: @cat %>

<br>

<div>
  <%= link_to "詳細", @cat %> |
  <%= link_to "戻る", cats_path %>
</div>
```


### 見た目を整える

app/controllers/cats_controller.rb

```ruby
      redirect_to @cat, notice: "ねこを登録しました。"

      redirect_to @cat, notice: "ねこを更新しました。"

	    redirect_to cats_url, notice: "ねこを削除しました。"
```

app/helpers/application_helper.rb

```ruby
module ApplicationHelper
  def sidebar_link_to(path, emoji, text)
    classes = %w[my-1 nav-link text-white]
    classes << "active" if current_page?(path)

    link_to(path, class: classes) do
      tag.span(class: "me-2") { emoji } + tag.span { text }
    end
  end

  def icon(icon_name)
    tag.i(class: ["bi", "bi-#{icon_name}"])
  end

  def icon_with_text(icon_name, text)
    tag.span(icon(icon_name), class: "me-2") + tag.span(text)
  end
end
```

`app/views/application/_flash.html.erb`

```html
<% if notice %>
  <div class="alert alert-primary alert-dismissible">
    <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
    <%= notice %>
  </div>
<% end %>

<% if alert %>
  <div class="alert alert-danger alert-dismissible">
    <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
    <%= alert %>
  </div>
<% end %>
```

`app/views/application/_sidebar.html.erb`

```html
<div class="d-flex flex-column flex-shrind-0 py-3 px-3 bg-dark text-white vh-100">
  <div class="d-flex align-items-center justify-content-center py-2">
    <%= link_to "AMS", root_path, class: "text-white display-6 text-decoration-name" %>
  </div>
  <hr />

  <ul class="nav nav-pills flex-column mb-auto">
    <li class="nav-item">
      <%= sidebar_link_to(cats_path, "🐱", "ねこ") %>
    </li>
    <li class="nav-item">
      <%= sidebar_link_to(dogs_path, "🐶", "いぬ") %>
    </li>
    <li class="nav-item">
      <%= sidebar_link_to(chicks_path, "🐣", "ひよこ") %>
    </li>
    <li class="nav-item">
      <%= sidebar_link_to(hedgehogs_path, "🦔", "ハリネズミ") %>
    </li>
    <li class="nav-item">
      <%= sidebar_link_to(owls_path, "🦉", "フクロウ") %>
    </li>
  </ul>
</div>
```

`app/views/cats/_cat.html.erb`

```html
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
```

app/views/cats/edit.html.erb

```html
<h1>ねこを編集する</h1>

<%= render "form", cat: @cat %>

<br>

<div>
  <%= link_to "詳細", @cat %> |
  <%= link_to "戻る", cats_path %>
</div>
```

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
  </div>
</div>
```

app/views/cats/new.html.erb

```html
<h1>New cat</h1>

<%= render "form", cat: @cat %>

<br>

<div>
  <%= link_to "戻る", cats_path %>
</div>
```

app/views/cats/show.html.erb

```html
<div id="<%= dom_id @cat %>">
  <p>
    <strong>名前:</strong>
    <%= @cat.name %>
  </p>
  <p>
    <strong>年齢:</strong>
    <%= @cat.age %>
  </p>
</div>

<div>
  <%= link_to "編集", edit_cat_path(@cat) %> |
  <%= link_to "戻る", cats_path %>
	<hr />
  <%= button_to "削除", @cat, method: :delete,  class: "btn btn-sm btn-outline-danger" %>
</div>
```


[[rails  ページネーション kaminari のデザインカスタマイズ]]

ここで、app/views/kaminari/ の中のファイルを設定する。


app/views/layouts/application.html.erb

```html
  <body>
    <div class="vh-100">
      <div class="container-fluid">
        <div class="row">
          <div class="ps-0" style="width: 200px;"></div>
          <div class="ps-0 position-fixed" style="width: 200px;">
            <%= render "sidebar" %>
          </div>

          <div class="col my-4">
            <%= render "flash" %>
            <%= yield %>
          </div>
        </div>
      </div>
    </div>
  </body>
```

config/routes.rb

```html
  root to: redirect("/cats")
```


![[Pasted image 20220518164647.png]]

