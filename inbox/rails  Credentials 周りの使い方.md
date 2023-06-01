#ruby/rails

---
2022-04-22  17:08

# rails  Credentials 周りの使い方
Production で動かしたい。
Secret key とかどーなってるのか。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/mby/items/08a70140e240d03ef877" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9UmFpbHMlRTMlODElQUVDcmVkZW50aWFscyVFMyU4MSVBRSVFNCVCRCU5QyVFMyU4MiU4QSVFNiU5NiVCOSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9OWM0NjgxMzk2MDM0OTg1ZTBlMzA5NDg3NTcwMzNiZjk&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwbWJ5JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz04OTQ4NDJhMTZiNGRhYTMwMGFkNmJmZjFmMGU1OTJjNw&blend-x=142&blend-y=491&blend-mode=normal&s=a826e95fbd55a190ad6762129e6946b0')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">RailsのCredentialsの作り方 - Qiita</h1>
		<p class="rich-link-card-description">
		Rails触った事ないけどある日突然production.keyを作成する事になり四苦八苦したので記録。 Railsのproduction.keyとは  production環境の暗号化されたファイルproduction.ymlを...
		</p>
		<p class="rich-link-href">
		https://qiita.com/mby/items/08a70140e240d03ef877
		</p>
	</div>
</a></div>

credentials.yml.enc と master.key がある。


既存キーの確認
```shell
$ rails credentials:show

# Used as the base secret for all MessageVerifiers in Rails, including the one protecting cookies.
secret_key_base: 5c1c9f4a4c15698b397c288c0f7b2029d9f7....
```

元々あるからいっか。

呼び出し方
```shell
$ rails c

irb(main):001:0> Rails.application.credentials.config
=> {:secret_key_base=>"5c1c9f4a4c15698b397c288c0f7b2029d9f76d2
```



ローカルサーバの立て方で、
```shell
$ rails s RAILS_ENV=production

ERROR
```

なので、こうする。
```shell
$ rails s -e production
```

うまく動いている。

### サーバの Nginx ではエラー

takizawanavi.jp/posts とアクセスすると

```
I,   INFO -- : [3b53a8d5-b543-435e-9461-49af77c5e1c0] Started GET "/posts" for 118.6.85.0 at 2022-04-22 17:25:19 +0900
I,   INFO -- : [3b53a8d5-b543-435e-9461-49af77c5e1c0] Processing by PostsController#index as HTML
I,   INFO -- : [3b53a8d5-b543-435e-9461-49af77c5e1c0]   Rendering posts/index.html.erb within layouts/application
D,  DEBUG -- : [3b53a8d5-b543-435e-9461-49af77c5e1c0]   Post Load (0.1ms)  SELECT "posts".* FROM "posts"
I,  INFO -- : [3b53a8d5-b543-435e-9461-49af77c5e1c0]   Rendered posts/index.html.erb within layouts/application (0.7ms)
I,   INFO -- : [3b53a8d5-b543-435e-9461-49af77c5e1c0] Completed 500 Internal Server Error in 3ms (ActiveRecord: 0.1ms)
F,  FATAL -- : [3b53a8d5-b543-435e-9461-49af77c5e1c0]
F,  FATAL -- : [3b53a8d5-b543-435e-9461-49af77c5e1c0] ActionView::Template::Error (The asset "application.css" is not present in the asset pipeline.
):
F,  FATAL -- : [3b53a8d5-b543-435e-9461-49af77c5e1c0]      5:     <%= csrf_meta_tags %>
      6:     <%= csp_meta_tag %>
      7:
      8:     <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
      9:     <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
     10:   </head>
     11:
F, FATAL -- : [3b53a8d5-b543-435e-9461-49af77c5e1c0]
F, FATAL -- : [3b53a8d5-b543-435e-9461-49af77c5e1c0] app/views/layouts/application.html.erb:8:in `_app_views_layouts_application_html_erb__2238086251086224949_12620'

```


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/aiandrox/items/408724ab8a4482fb5873" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9JUUzJTgwJTkwUmFpbHMlRTMlODAlOTElRTYlOUMlQUMlRTclOTUlQUElRTclOTIlQjAlRTUlQTIlODMlRTMlODElQUIlRTMlODElOEElRTMlODElOTElRTMlODIlOEIlRTMlODIlQTIlRTMlODIlQkIlRTMlODMlODMlRTMlODMlODglRTMlODMlOTclRTMlODMlQUElRTMlODIlQjMlRTMlODMlQjMlRTMlODMlOTElRTMlODIlQTQlRTMlODMlQUIlRTMlODElQUUlRTglQTglQUQlRTUlQUUlOUEmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT01NiZ0eHQtY2xpcD1lbGxpcHNpcyZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPWM0YmMzY2E4YzhhZTE0ZGFjYmY0YWQ0Y2IxNzIwODM5&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwYWlhbmRyb3gmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTMwYjMwMjg2ODVmMmM2MTI1MjM0YTc3NmMwMTZlM2I0&blend-x=142&blend-y=491&blend-mode=normal&s=4f3e93522e674d388da6fc7c7bcc8b85')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">【Rails】本番環境におけるアセットプリコンパイルの設定 - Qiita</h1>
		<p class="rich-link-card-description">
		自分用メモです。 本番環境を整えた後のアセットプリコンパイルの設定について。 環境  ruby 2.6.4 Rails 5.2.4.1 puma 3.12.4 nginx 1.12.2 エラーログ  本番環境でルートにアクセス...
		</p>
		<p class="rich-link-href">
		https://qiita.com/aiandrox/items/408724ab8a4482fb5873
		</p>
	</div>
</a></div>


assets のプリコンパイルが無いよ、らしい。

```shell
$ ll public/assets/

application-35729bfbaf9967f119234595ed222f7ab14859f304ab0acc5451afb387f637fa.css
....
```

ブラウザで確認する。

takizawanavi.jp/assets/application-35729bfbaf9967f119234595ed222f7ab14859f304ab0acc5451afb387f637fa.css

![[Pasted image 20220422173633.png]]

ある。

ここをいじればいいとのこと。

config/environments/production.rb

```ruby
  #config.public_file_server.enabled =  ENV['RAILS_SERVE_STATIC_FILES'].present?
  config.public_file_server.enabled = true
```

![[Pasted image 20220422174345.png]]

credential 周り全然出てこないまま、Production モードで Nginx 公開出来てしまった。


