#ruby/rails 


[Active Storage の概要 - Railsガイド](https://railsguides.jp/active_storage_overview.html)

# 簡単な例でやってみる

```shell
$ rails active_storage:install
$ rails g scaffold user name


$ rails db:migrate
```

active_storage_blobs と　active_storage_attachements というテーブルが作られる。

ストレージサービスの設定は config/storage.yml にある。

環境ごとに使うストレージサービスは config/environments/ の各ファイルに

```ruby
config.active_storage.service = :local
```

などと設定する。

## Model に設定

```ruby
class User < ApplicationRecord
  
  has_one_attached :portrait
end
```

## View に設定

### \_form.html.erb

```ruby
  <div class="field">
    <%= form.label :name %>
    <%= form.text_field :name %>
  </div>

  <div class="field">
    <%= form.label :portrait %>
    <%= form.file_field :portrait %>
  </div>

  <div class="actions">
    <%= form.submit %>
  </div>
```

### show.html.erb

```ruby
<p id="notice"><%= notice %></p>

<p>
  <strong>Name:</strong>
  <%= @user.name %>
</p>

<p>
  <strong>Portrait:</strong>
  <%= image_tag @user.portrait if @user.portrait.attached? %>
  
  <%= link_to "Download", rails_blob_path(@user.portrait, disposition: "attachment") %>
</p>

<%= link_to 'Edit', edit_user_path(@user) %> |
<%= link_to 'Back', users_path %>
```

これだけ。
モデルにだけ設定して、テーブルにカラムが要らない。

## ダウンロードされるには

```ruby
  <%= link_to "Download", rails_blob_path(@user.portrait, disposition: "attachment") %>
```

Download に disposition: "attachment" 指定すると、ページはそのままでブラウザの下にダウンロードのバナーが出ていつものダウンロードスタイルになる。
(URLに ?disposition=attachment とくっ付く)

## モデルの削除とともにファイルを削除するには?

destroy コールバックで user.portrait.purge を呼んでみる。

```ruby
class User < ApplicationRecord

  has_one_attached :portrait

  before_destroy :purge_portrait

  def purge_portrait
    self.portrait.purge
  end
end
```

これで active_storage の２つのテーブルからデータは削除され、添付ファイルは削除された。
が、フォルダーは空のままのこったままだ。まあいいか。

* * *
