#ruby/rails 




[form objectを使ってみよう - メドピア開発者ブログ](https://tech.medpeer.co.jp/entry/2017/05/09/070758)

## DB を使わないでメール返信するコード

```ruby
class Feedback
  include activeModel::Model
  
  attr_accessor :title, :body
  
  validates :title, :body, presence: true
  
  def save
    return false if invalid?
    AdminMailer.feedback(title, body).deliver_later
    true
  end
end
```

```ruby
class FeedbacksController < ApplicationController
  
  def new
    @feedback = Feedback.new
  end
  
  def create
    @feedback = Feedback.new(feedback_params)
    if @feedback.save
      redirect_to home_path, notice: 'フィードバッグを返信しました'
    else
      render :new
    end
  end

  def feedback_params
    params.require(:feedback).permit(:title, :body)
  end
end
```

```ruby
<%= form_with model: @feedback, local: true do |f| %>
  <% if @feedback.errors.any? %>
    <% @feedback.errors.full_messages.each do |message| %>
      <%= message %>
    <% end %>
  <% end %>
  
  <%= f.label :title %>
  <%= f.text_field :title %>
  <%= f.label :body %>
  <%= f.text_area :body %>
  <%= f.submit %>
<% end %>

```

## DB を絡ませる

```ruby
class Signup
  include ActiveModel::Model
  
  attr_accessor :email, :password, :password_confirmation
  
  validates :email,
            presence: true,
            format: { with: /\A.+@.+\z/ }
  validates :password, presende: true, length: { minimum: 6 }, confiration: { allow_blank: true }
  
  def save
    return false if invalid?
    
    user = User.new(email: email, password: password, password_confirmation: password_confirmation)
    user.save!
    UserMailer.welcome(user).deliver_later
    true
  end
end
```

```ruby
class User < ApplicationRecord
  has_secure_password
end
```

### 項目にチェックボックスを使う

属性のキャストは自分でするとのこと。

```ruby
class Signup
  attr_accerror :email, :password, :password_confirmation, :accept_dm
  
  def accept_dm
    Activerecord==Type==Boolean.new.cast(@accept_dm)
  end
end


signup = Signup.new(accept_dm: '1')
signup.accept_dm       #=> true

signup = Signup.new(accept_dm: '0')
signup.accept_dm       #=> false
```
