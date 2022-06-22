#elixir #js/vue 

---
2021-07-15

# Phoenix + Vue.js入門

## Phonix で JSON API を作成

```shell
$ mix phx.new phoenix_vue_example
$ cd phonix_vue_example
$ mix ecto.create
```

Postgresql がデフォルトなので、入れてセットアップした過程がこちら #postgresql  インストール]]

PostgreSQL のデータベースを見る

```shell
$ psql postgres -l

                                  List of databases
          Name           |  Owner   | Encoding | Collate | Ctype | Access privileges
-------------------------+----------+----------+---------+-------+-------------------
 phoenix_for_rails_dev   | george   | UTF8     | C       | C     |
 phoenix_vue_example_dev | postgres | UTF8     | C       | C     |
 postgres                | george   | UTF8     | C       | C     |
 template0               | george   | UTF8     | C       | C     | =c/george        +
                         |          |          |         |       | george=CTc/george
 template1               | george   | UTF8     | C       | C     | =c/george        +
                         |          |          |         |       | george=CTc/george
(5 rows)

```

ある。

スキャフォールドする。

```shell
$ mix phx.gen.json Blog Article articles title:string body:text
```

ルーティングの追加。

lib/phonix_vue_example_web/router.ex

```elixir

    # plug :protect_from_forgery       # コメントアウト
    
 scope "/", PhoenixVueExampleWeb do
    pipe_through :browser

    get "/", PageController, :index
    resources "/articles", ArticleController, except: [:new, :edit]   # 追加
    
```

マイグレートする。
```shell
$ mix ecto.migrate

$ psql postgres -d phoenix_vue_example_dev -c "\d articles"

                                          Table "public.articles"
   Column    |              Type              | Collation | Nullable |               Default
-------------+--------------------------------+-----------+----------+--------------------------------------
 id          | bigint                         |           | not null | nextval('articles_id_seq'::regclass)
 title       | character varying(255)         |           |          |
 body        | text                           |           |          |
 inserted_at | timestamp(0) without time zone |           | not null |
 updated_at  | timestamp(0) without time zone |           | not null |
Indexes:
    "articles_pkey" PRIMARY KEY, btree (id)
```


ルーティングの確認。

```shell
$ mix phx.routes

          page_path  GET     /                                      PhoenixVueExampleWeb.PageController :index
       article_path  GET     /articles                              PhoenixVueExampleWeb.ArticleController :index
       article_path  GET     /articles/:id                          PhoenixVueExampleWeb.ArticleController :show
       article_path  POST    /articles                              PhoenixVueExampleWeb.ArticleController :create
       article_path  PATCH   /articles/:id                          PhoenixVueExampleWeb.ArticleController :update
                     PUT     /articles/:id                          PhoenixVueExampleWeb.ArticleController :update
       article_path  DELETE  /articles/:id                          PhoenixVueExampleWeb.ArticleController :delete
live_dashboard_path  GET     /dashboard                             Phoenix.LiveView.Plug :home
live_dashboard_path  GET     /dashboard/:page                       Phoenix.LiveView.Plug :page
live_dashboard_path  GET     /dashboard/:node/:page                 Phoenix.LiveView.Plug :page
          websocket  WS      /live/websocket                        Phoenix.LiveView.Socket
           longpoll  GET     /live/longpoll                         Phoenix.LiveView.Socket
           longpoll  POST    /live/longpoll                         Phoenix.LiveView.Socket
          websocket  WS      /socket/websocket                      PhoenixVueExampleWeb.UserSocket
```

http://localhost:4000/articles

POST してみる。
```shell
$ curl -X POST -H "Content-Type: application/json" -d '{"article":{"title":"素晴らしいタイトル", "body":"素晴らしい本文"}}' localhost:4000/articles
```

GET してみる。
```shell
$ curl -X 'GET' localhost:4000/articles
```

## Vue.js で画面表示する

priv/static/js/app.js のケツに素で追記

```js

var app = new Vue({
  el: '[role="main"]',
  data: {
    articles: [],
  },
  created: function () {
    axios.get('/articles').then((response) => {
      app.articles = response.data.data
    })
  },
})
```

vue と axio を CDN で持ってくる。

lib/phoenix_vue_example_web/templates/layout/app.html.eex

```html
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
```

lib/phoenix_vue_example_web/templates/page/index.html.eex

```html
<section class="phx-hero">
  <h1><%= gettext "Welcome to %{name}!", name: "Phoenix" %></h1>
  <p>Peace of mind from prototype to production</p>
</section>

<section class="row">
  <div v-for="article in articles">
    <h2>{{ article.title }}</h2>
    <pre>{{ article.body }}</pre>
  </div>
</section>
```


![[Pasted image 20210715144720.png]]

残念。

システム内で仲良く Vue.js がいるわけじゃないのか。
