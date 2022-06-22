#ruby/rails 


# Devise を入れる

[\[_Rails_\] deviseの使い方（rails5版） - Qiita](https://qiita.com/cigalecigales/items/f4274088f20832252374)

```ruby
$ rails new devise_sample
$ cd devise_sample
```

```ruby
[[Gemfile]]
gem 'devise'
gem 'devise-i18n'
gem 'devise-i18n-views'
```

```ruby
$ bundle install
$ rails g devise:install

$ rails g devise:views:locale ja
```

```shell
===============================================================================

Depending on your application's configuration some manual setup may be required:

  1. Ensure you have defined default url options in your environments files. Here
     is an example of default_url_options appropriate for a development environment
     in config/environments/development.rb:

       config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

     In production, :host should be set to the actual host of your application.

     * Required for all applications. *

  2. Ensure you have defined root_url to *something* in your config/routes.rb.
     For example:

       root to: "home#index"

     * Not required for API-only Applications *

  3. Ensure you have flash messages in app/views/layouts/application.html.erb.
     For example:

       <p class="notice"><%= notice %></p>
       <p class="alert"><%= alert %></p>

     * Not required for API-only Applications *

  4. You can copy Devise views (for customization) to your app by running:

       rails g devise:views

     * Not required *

===============================================================================
```

```shell
$ rails g controller Pages index show
```

# config/routes.rb

```ruby
root 'pages#index'
get 'pages/show'
```

## flashメッセージ

# app/views/layouts/application.rb

```ruby

  <body>
    <p class="notice"><%= notice %></p>
    <p class="alert"><%= alert %></p>
    
    <%= yield %>
  </body>
```

### Devise View 生成

```shell
$ rails g devise:views
```

## User モデルの作成

```shell
$ rails g devise User
$ rails db:migrate
```

# app/views/layouts/application.html.erb

```ruby
<body>
    <header>
      <nav>
        <% if user_signed_in? %>
          <strong><%= link_to current_user.email, pages_show_path %></strong>
          <%= link_to 'プロフィール変更', edit_user_registration_path %>
          <%= link_to 'ログアウト', destroy_user_session_path, method: :delete %>
        <% else %>
          <%= link_to 'サインアップ', new_user_registration_path %>
          <%= link_to 'ログイン', new_user_session_path %>
          <% end %>
      </nav>
    </header>
    <p class="notice"><%= notice %></p>
    <p class="alert"><%= alert %></p>

    <%= yield %>
 </body>
```

localhost:3000

![](welcome.png)

サインアップしてみる。
もちろんサインアップ前にログインしても入れない。

![](sign_up.png)

基本的に認証動作するようだ。

# ソースを見る
