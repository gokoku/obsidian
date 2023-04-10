---
type: note
---

#ruby/rails 

---
2022-12-07  08:51

# rails Render にデプロイ出来る?!

Heroku から Render へ移ることにした。

https://dashboard.render.com/register

<div class="rich-link-card-container"><a class="rich-link-card" href="https://dashboard.render.com/register" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://render.com/og-img.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Cloud Application Hosting for Developers | Render</h1>
		<p class="rich-link-card-description">
		Render is a unified cloud to build and run all your apps and websites with free SSL, global CDN, private networks and automatic deploys from Git.
		</p>
		<p class="rich-link-href">
		https://dashboard.render.com/register
		</p>
	</div>
</a></div>

GitHub で登録しよう。

local に postgresql を入れよう。
[[postgresql. Mac に install]]


<div class="rich-link-card-container"><a class="rich-link-card" href="https://render.com/docs/deploy-rails" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://render.com/og-img.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Getting Started with Ruby on Rails on Render | Render</h1>
		<p class="rich-link-card-description">
		This guide demonstrates how to set up a local Ruby on Rails environment, create a simple view, and deploy it to Render.
		</p>
		<p class="rich-link-href">
		https://render.com/docs/deploy-rails
		</p>
	</div>
</a></div>

この手順でいく。

```shell
予め準備
$ gem install pg

$ rails new render-sample --database=postgresql

$ cd render-sample
$ rails g controller Render index
```

config/routes.rb
```ruby
Rails.application.routes.draw do
  get 'render/index'
  root "render#index"
end
```

app/views/render/index.html.erb
```html
<main class="container">
  <div class="row text-center justify-content-center">
    <div class="col">
      <h1 class="display-4">Hello World!</h1>
    </div>
  </div>
</main>
```

Bootstrap を追加する。

```shell
$ yarn add bootstrap
$ mv app/assets/stylesheets/application.css app/assets/stylesheets/application.scss
```

app/assets/stylesheets/application.scss
```scss
@import "bootstrap/scss/bootstap";
@import "**/*";
```

これだけだと 7 はダメで、gem と生成が必要。
Gemfile
```ruby
gem 'cssbundling-rails'
```

```shell
$ bundle install
$ rails css:install:bootstrap

サーバ起動　scss コンパイル
$ bin/dev
```

app/views/layouts/application.html.erb
```html
[<!DOCTYPE html>
<html lang="<%= I18n.locale %>">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="with=device-width, initial-scale=1.0" />](<%3C!DOCTYPE html%3E
<html lang="<%= I18n.locale %>">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="with=device-width, initial-scale=1.0" />

    <title>RenderSample</title>

    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag "application", "data-turbo-track": "reload" %>
    <%= javascript_importmap_tags %>
  </head>

  <body>
    <header class="container mt-4 mb-4">
      <a href="https://render.com">
        <%= image_tag 'render.png', class: 'mw-100' %>
      </a>
    </header>
    <%= yield %>
  </body>
</html>>)
```

バナーを app/assets/images/render.png に設置する。
![[render-banner.png]]



![[Pasted image 20221207095739.png]]

## 次の準備

config/database.yml
```ruby
production:
  <<: *default
  url: <%= ENV["DATABASE_URL"] %>
```

config/puma.rb
```ruby
#worker_timeout 3600 if ENV.fetch("RAILS_ENV", "development") == "development"

workers ENV.fetch("WEB_CONCURRENCY") { 4 }
preload_app!

```

config/environments/production.rb
```ruby
 config.public_file_server.enabled = ENV["RAILS_SERVE_STATIC_FILES"].present? || ENV['RENDER'].present?
```

shell script を作る。
bin/render-build.sh
```shell
#!/usr/bin/env bash
# exit on error
set -o errexit

bundle install
bundle exec rake assets:precompile
bundle exec rake assets:clean
bundle exec rake db:migrate
```

```shell
$ chmod a+x bin/render-build.sh
```

./render.yaml
最終的に、リージョンはシンガポール、とか、free とか
```yaml
databases:
  - name: render_sample
    databaseName: render_sample
    user: render_sample
    region: singapore

services:
  - type: web
    name: render_sample
    env: ruby
    region: singapore
    plan: free
    buildCommand: './bin/render-build.sh'
    startCommand: "bundle exec puma -C config/puma.rb"
    envVars:
      - key: DATABASE_URL
        fromDatabase:
          name: render_sample
          property: connectionString
      - key: RAILS_MASTER_KEY
        sync: false
```

とりあえず GitHub にupする。

![[Pasted image 20221207102539.png]]

# render.com

![[Pasted image 20221207102635.png|500]]

web service を作る。

GitHub とコネクトすると
![[Pasted image 20221207102849.png|400]]

ここでリポジトリを選ぶ。

サーバはシンガポールを選んだ。

create

![[Pasted image 20221207103301.png|300]]
失敗

Environment を設定する。 RAILS_MASTER_KEY
![[Pasted image 20221207103543.png|600]]

mater.key は config 

![[Pasted image 20221207103657.png|500]]
データベースを作る

render.yaml と同じ名前で作った。
フリーは90日でデリートとのこと。



プラットフォーム指定しておくといいらしい。
```shell
$ bundle lock --add-platform x86_64-linux
```

config/applicaton.rb に host 名を書くらしい。
```ruby
module RenderSample
  class Application < Rails::Application
	config.hosts << 'render-sample-n4b8.onrender.com'
```

![[Pasted image 20221207105555.png|500]]

コミットすると自動で render が走る。

![[Pasted image 20221207112009.png]]

これ成功か!!

リンクから https://render-sample-n4b8.onrender.com/

![[Pasted image 20221207112040.png]]

成功だ！
