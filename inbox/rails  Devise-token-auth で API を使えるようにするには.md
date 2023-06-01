---
type: note
---

#ruby/rails 

---
2022-05-20  15:12

# rails  Devise-token-auth で API を使えるようにするには


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/tomokazu0112/items/5fdd6a51a84c520c45b5" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9JUUzJTgwJTkwUmFpbHMlMjBBUEklMjAlRTUlODUlQTUlRTklOTYlODAlRTMlODAlOTFkZXZpc2UtdG9rZW4tYXV0aCZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9NmM1NDZmMDQyNGIyMmVkMDc1ZGNlMTRmZGJhMWZiYjk&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwdG9tb2thenUwMTEyJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz01YjkyMjkyNDJmMjRhOGNhNjJlZjEwMDU4ZWQ4ZmI4YQ&blend-x=142&blend-y=491&blend-mode=normal&s=c061cdbe600fcb891ab618198b9f6fba')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">【Rails API 入門】devise-token-auth - Qiita</h1>
		<p class="rich-link-card-description">
		はじめに devise-token-auth とは？  Railsにおけるトークン認証を実現するgemです。  このdevise-token-authを用いることで、新規登録、ログイン・ログアウトなどはもちろんのこと、アプリ各...
		</p>
		<p class="rich-link-href">
		https://qiita.com/tomokazu0112/items/5fdd6a51a84c520c45b5
		</p>
	</div>
</a></div>


これで入門してみよう。

```shell
$ rails new token-auth --api -d mysql

$ cd token-auth
```

pop-os chromebook で mariadb を立ち上げる。

config/database.yml

```yaml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: root
  host: pop-os.local
```

```shell
$ rails db:create
```

Gemfile

```ruby
gem "devise"
gem "devise_token_auth", git: "https://github.com/lynndylanhurley/devise_token_auth" # rubygemsだとまだRails7に対応していない
gem "rack-cors"   #<--コメントインする
```

```shell
$ bundle install
$ rails g devise:install
$ rails g devise_token_auth:install User auth
$ rails db:migrate
```

## 認証機能実装

```shell
$ rails g controller api/v1/auth/registrations
```

app/controllers/api/v1/auth/registrations

```ruby
class V1::Auth::RegistrationsController < DeviseTokenAuth::RegistrationsController
```

config/routes.rb

```ruby
Rails.application.routes.draw do
  namespace :api do
    scope :v1 do
      mount_devise_token_auth_for 'User', at: 'auth'
    end
  end

  # Defines the root path route ("/")
  # root "articles#index"
end
```
localhost:3000/api/vi/auth
これで認証が実装されるのだそうです。

Userモデルの :trackable をコメントアウトする(ログイン時にエラーが発生するそうです)
ってなってた。

```ruby
class User < ActiveRecord::Base
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable
  include DeviseTokenAuth::Concerns::User
end
```
## api モードでのセッションを有効にする!!
これで引っかかってた。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://daido.hatenablog.jp/entry/2020/05/06/143145" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Rails の API モードでセッションを有効にする - Note</h1>
		<p class="rich-link-card-description">
		巷には config/application.rb 内で Rails.app_class.config.api_only = false にすればできる、的な記事が溢れているが、不要な middleware まで読み込みたくない。 環境 ruby 2.7.1 rails 6.0.3 差分 デフォルトの CookieSt…
		</p>
		<p class="rich-link-href">
		https://daido.hatenablog.jp/entry/2020/05/06/143145
		</p>
	</div>
</a></div>



config.api_only = true を false にする。

config/applicsation.rb

```ruby
    # Only loads a smaller set of middleware suitable for API only apps.
    # Middleware like session, flash, cookies can be added back manually.
    # Skip views, helpers and assets when generating a new resource.
    config.api_only = false
  end
end
```



### 使い方 Usage
<div class="rich-link-card-container"><a class="rich-link-card" href="https://devise-token-auth.gitbook.io/devise-token-auth/usage" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://app.gitbook.com/share/space/thumbnail/-L9vQNRBNEYkwzEL3fSh/page/-L9vRAlEVub2-lO7o3u9.png?color=%2350c0c4&logo=https%3A%2F%2F4076698016-files.gitbook.io%2F~%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fspaces%252F-L9vQNRBNEYkwzEL3fSh%252Favatar.png%3Fgeneration%3D1523562941742335%26alt%3Dmedia&theme=')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Usage</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://devise-token-auth.gitbook.io/devise-token-auth/usage
		</p>
	</div>
</a></div>
##### 新規登録

/api/v1/auth
POST
情報 : emal, password

```shell
$ curl -X POST -H "Content-Type: application/json" -d `{"email": "admin@example.com", "password": "password"}` http://localhost:3000/api/v1/auth

{"status":"success","data":{"email":"admin@example.com","provider":"email","uid":"admin@example.com","id":1,"allow_password_change":false,"name":null,"nickname":null,"image":null,"created_at":"2022-05-20T06:46:55.145Z","updated_at":"2022-05-20T06:46:55.658Z"}}
```

![[Pasted image 20220520155121.png | 500]]

なんと！ユーザが登録された!!


##### ログイン

/api/v1/auth/sign_in
POST
情報 : emal, password

```shell
$ curl -i -X POST -H "Content-Type: application/json" -d `{"email": "admin@example.com", "password": "password"}` http://localhost:3000/api/v1/auth/sign_in

HTTP/1.1 200 OK
access-token: IU_8g23iVq_aQ7y3WjqXTA
client: ONN1tGaWXOEpx4siWZplOA
uid: admin@example.com

{"data":{"email":"admin@example.com","provider":"email","uid":"admin@example.com","id":1,"allow_password_change":false,"name":null,"nickname":null,"image":null}}%
```

uid: admin@example.com
client : ONN1tGaWXOEpx4siWZplOA
access-token: IU_8g23iVq_aQ7y3WjqXTA

これを使ってサインアウトするらしい。

##### サインアウト

/api/v1/auth/sign_out
DELETE
情報 : uid, access-token, client

```shell
$ curl -i -X DELETE -H "Content-Type: application/json" -d '{"uid":"admin@example.com", "client": "ONN1tGaWXOEpx4siWZplOA", "access-token": " IU_8g23iVq_aQ7y3WjqXTA"}' http://localhost:3000/api/v1/auth/sign_out
```


##### アカウント削除
サインインしてからじゃないとな。

/api/v1/auth/
DELETE
情報 : uid, access-token, client

```shell
$ curl -i -X DELETE -H "Content-Type: application/json" -d '{"uid": "admin@example.com", "access-token": "KdB4u3FmFXYDLlT8A1_a5w", "client": "kLeNLF5sOdmRJdqd41A4pA"}' http://localhost:3000/api/v1/auth/

{"status":"success","message":"Account with UID 'admin@example.com' has been destroyed."}%
```

上手く行った!!!



これで、なんかコントローラを動かせればいいわけか。

子機の方ではログインして、トークンとかを取得して、それを使ってコントローラを使うとのこと。

