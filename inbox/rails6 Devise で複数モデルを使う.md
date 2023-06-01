#ruby/rails 



[devise の複数モデル管理を試みる | Octo's blog](https://ccbaxy.xyz/blog/2020/03/20/ruby33/[[xian-zai-noruteingu]])

## セットアップ

admin ユーザと user ユーザを用意したい。

    gem 'devise'
    gem 'devise-i18n

```ruby
$ rails g devise:install
```

config/initializers/devise.rb

```ruby
[[複数model]]で個別ログイン画面を使えるようにするらしい
config.scoped_views = true

[[複数model]]で、一方をログアウトしたときにもう片方をログアウトすることを防ぐ
config.sign_out_all_scopes = false
```

## 生成

### modelの生成

```ruby
$ rails g devise user
$ rails g devise admin

$ rails db:migarte
```

### controller の生成

```ruby
$ rails g devise:controllers users
$ rails g devise:controllers admins
```

### view の生成

```ruby
$ rails g devise:views users
$ rails g devise:views admins
```

## ルーチングの修正

devise_for :admins
devise_for :users

今の状態は、こうなって丸かぶりしてる状態。

```ruby

  devise_for :managers, controllers: {
    sessions: 'managers/sessions',
    passwords: 'admins/passwords',
    registrations: 'admins/registrations'
  }

  devise_for :users, controllers: {
    sessions: 'users/sessions',
    passwords: 'users/passwords',
    registrations: 'users/registrations'
  }
```

## view リンク

判定
current_namager
current_user

ex.
login: new_manager_session_path
logout: destroy_manager_session_path
