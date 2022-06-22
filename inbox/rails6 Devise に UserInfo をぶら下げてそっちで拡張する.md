#ruby/rails 



Devise で User を自動で作ってもらうのはいいが、ユーザー情報の拡張・縮小・カスタマイズがやりにくい。

そこで、User とユーザー情報モデルを分離して1対1リレーションで実装することにする。

# Devise で User を用意する

```shell
# devise のモデルを作る
$ rails g devise User
# devise のコントローラを作る
$ rails g devise:controllers users
# devise のView を用意する
$ rails g devise:view users

$ rails db:migrate
```

# UserInfo を用意する

User には references で設定するほうが良しなにしてくれるとのこと。

```shell
$ rails g model UserInfo name:string address:string user:references

$ rails db:migrate
```

## Model

1 対 1 なので、単数。

```ruby
class User < ApplicationRecord
  has_one :user_info, dependent: :destroy
end
```

```ruby
class UserInfo < ApplicationRecord
  belongs_to :user
end
```

## Controller

app/controller/users/ragistrations_controller.rb

```ruby

# sign_up 
  def new
    @user_info = UserInfo.new
    super
  end

  def create
    super
    resource.create_user_info(user_info_params)
  end








  def user_info_params
    params.require(:user).require(:user_info).permit(:name, :address)
  end
```

## View

複数モデルのフォームを実装する

sign_up

vews/users/registrations/new.html.slim

```ruby
= form_for resource, as: resource_name, url: registration_path(resource_name) do |f|
  = f.label :email
  = f.email_field :email

  = f.label :password
  = f.password_field :password
  
  = f.label :password_confirmation
  = f.password_field :password_confirmation
  
  = f.fields_for @user_info do |i|
    = i.label :name
    = i.text_field :name
    
    = i.label :address
    = i.text_field :address
  
  = f.submit t(sign_up)
```
