#ruby/rails

---
2022-01-05

# 久しぶりに入門

devise + cancancan + rolify でいってみる。

```shell
$ rails new sample
$ cd sample

$ rails g scaffold Post content:string
$ rails db:migrate
```
Gemfile

```
gem 'devise'
gem 'cancancan'
gem 'rolify'
```

#### devise

```shell
$ rails g devise:install
```

言われた通りにやってみる。

config/environments/development.rb

```ruby
  config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

config/routes.rb

```ruby
   root to: "posts#index"
```

app/views/layouts/application.html.erb

```html
  <body>
    <header>
      <nav>
          <% if user_signed_in? %>
            <strong><%= link_to posts_path %></strong>
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
```shell
$ rails g devise:views

$ rails g devise User
$ rails db:migrate
```

app/controllers/application_controller.rb

```ruby
class ApplicationController < ActionController::Base

  protect_from_forgery with: :exception

  before_action :authenticate_user!

  before_action :configure_permitted_parameters, if: :devise_controller?

  protected
  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:name])
    devise_parameter_sanitizer.permit(:account_update, keys: [:name])
  end

end
```

Devise のローカライズファイルの取得。

```shell
$ wget https://gist.githubusercontent.com/satour/6c15f27211fdc0de58b4/raw/d4b5815295c65021790569c9be447d15760f4957/devise.ja.yml -P config/locales/
```


#### cancancan と rolify

```shell
$ rails g cancan:ability

$ rails g rolify Role User

$ rails db:migrate
```

app/models/ability.rb

```ruby
def initialize(user)

   if user.has_role? :admin
      can :manage, :all
   else
      can :read, :all
   end
```

admin ロールの出来ることを設定する。

user  に admin を紐付ける。

```shell
$ rails c

>> user = User.new
>> user.email = "test@example.com"
>> user.password = "test1234"       # 短いと引っ掛かるよ
>> user.save


>> user.add_role "admin"

>> ability = Ability.new(user)
>> ability.can? :manage, :all
>> true
```

admin だけに Edit Destroy を使えるようにしてみる。

app/views/posts/index.html.erb

```html
  <tbody>
    <% @posts.each do |post| %>
      <tr>
        <td><%= post.content %></td>
        <td><%= link_to 'Show', post %></td>
        <td>
          <% if current_user.has_role? :admin %>
            <%= link_to 'Edit', edit_post_path(post) %>
          <% end %>
        </td>
        <td>
          <% if current_user.has_role? :admin %>
            <%= link_to 'Destroy', post, method: :delete, data: { confirm: 'Are you sure?' } %></td>
          <% end %>
      </tr>
    <% end %>
  </tbody>
```


![[Pasted image 20220105140046.png]]

admin の付いてないユーザーでは、

![[Pasted image 20220105140146.png]]

https://github.com/RolifyCommunity/rolify/wiki/Devise---CanCanCan---rolify-Tutorial


最近は cancancan ではなく pundit なのかな??

##### ロールを操作するには

https://github.com/RolifyCommunity/rolify


remove_role とかある。

