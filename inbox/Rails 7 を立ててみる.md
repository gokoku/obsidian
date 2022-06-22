---
type: note
---

#ruby/rails

---
2022-05-17  10:05

# Rails 7 を立ててみる

ここを参考にする。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/tikaranimaru/items/70f87b1fe4b1e7166348" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9UmFpbHM3JUUzJTgyJTkyJUU2JUE3JThCJUU3JUFGJTg5JUUzJTgxJTk3JUUzJTgxJUE2JUUzJTgxJUJGJUUzJTgyJThCJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz03YWZjYTg5MjVkNDI3MjkzNWQ3M2EzOGEzNzQwYTVjOA&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwdGlrYXJhbmltYXJ1JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz00ZmRhYzU2YmJiMzU2NDliZWZlZmY3YTRlZmU0ZDc1MA&blend-x=142&blend-y=491&blend-mode=normal&s=29321e4c3ee7edefc341590df5065f50')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Rails7を構築してみる - Qiita</h1>
		<p class="rich-link-card-description">
		Rails7がリリースされたので、ざっくり立ち上げまで試してた。 Rail7をインストール  $ mkdir trial-rais7 $ cd trial-rais7/  # rubyのバージョンを2.7以上にする $ rbe...
		</p>
		<p class="rich-link-href">
		https://qiita.com/tikaranimaru/items/70f87b1fe4b1e7166348
		</p>
	</div>
</a></div>



```shell
$ rails new trial-rails7 --css=tailwind --database=mysql --force
```

#### pop-os で MariaDB を docker で立てる

```shell
$ cd ~/docker/mariadb_10
$ docker compose up
```

データベースの設定は、host に pop-os.local を書く。

Gemfile

```ruby
gem "mysql2"
```


config/database.yml

```yaml
# SQLite. Versions 3.8.0 and up are supported.
#   gem install sqlite3
#
#   Ensure the SQLite 3 gem is defined in your Gemfile
#   gem "sqlite3"
#
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: root
  host: pop-os.local

development:
  <<: *default
  database: trial-rails7-development

test:
  <<: *default
  database: trial-rails7-test
  
production:
  <<: *default
  database: trial_rails7_production
  username: trial_rails7
  password: <%= ENV["TRIAL_RAILS7_DATABASE_PASSWORD"] %>

```
```shell
$ rails db:create
$ rails s
```

![[Pasted image 20220517103019.png]]


tailwind製のUIコンポーネント daisy UI を入れるそうです。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://daisyui.com/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://daisyui.com/images/default.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">daisyUI — Tailwind CSS Components</h1>
		<p class="rich-link-card-description">
		Tailwind Components Library - Free components for Tailwind CSS
		</p>
		<p class="rich-link-href">
		https://daisyui.com/
		</p>
	</div>
</a></div>




```shell
$ yarn add daisyui
```

config/tailwind.config.js

```js
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/aspect-ratio'),
    require('@tailwindcss/typography'),
    require('daisyui'),
  ]
```

app/views/layouts/application.html.erb

```heml
<!DOCTYPE html>
<html data-theme="retro">
  <head>
    <title>TrialRails7</title>
```

scaffold

```shell
$ rails g scaffold kind name:string
$ rails g scaffold food name:string kind:references price:integer memo:text is_deleted:boolean deleted_at:datetime

$ rails db:mingrate
```


foreman で起動するらしい。

```shell
$ foreman start -f Procfile.dev
```
Procfile.dev で tailwindcss の watch を一緒に走らせるのか。

![[Pasted image 20220517105109.png | 300]]

なんと！すでにこうなる。

Gemfile

```ruby
gem "ransack"   # 検索を良い感じにしてくれるらしい
gem "kaminari"  #ページネーション
```




