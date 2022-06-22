#ruby/rails 



```shell
$ rails new tono_reservation_system

```

Gemfile の追加はこんな感じ

```ruby
gem 'slim-rails'
gem 'html2slim', require: false
gem 'rails-i18n'
gem 'devise'
gem 'devise-i18n'
gem 'activeadmin'
gem 'dotenv-rails'
```

とりあえず Scaffold する。

```shell
$ rails g scaffold Booking data:text
$ rails db:migrate
$ rails db:migrate RAILS_ENV=test
```

## 予約タイムテーブル

Booking は予約テーブル

![](24ee653e-5b1d-4db5-a528-573987e69d78-klwduuei.png)


## 予約するものたち

借りられるものは全て Reservable として、
親子関係を持たせられるようにする。

会議室A に備品たちが属する形に出来る。

[Railsでツリー構造（階層構造）をもったカテゴリを隣接リストモデルで実装する - Qiita](https://qiita.com/yuyasat/items/1200d7a6b56bae0c6f57)

[Railsで木構造を扱うには｜TechRacho（テックラッチョ）〜エンジニアの「？」を「！」に〜｜BPS株式会社](https://techracho.bpsinc.jp/hira/2018_03_15/53872)



[amerine/acts_as_tree: ActsAsTree -- Extends ActiveRecord to add simple support for organizing items into parent–children relationships.](https://github.com/amerine/acts_as_tree)

Gemfile

```
gem 'acts_as_tree

```

```shell
$ rails g model Reservable name:string price:integer parent_id:integer
```

migrate file

```ruby
class CreateReservables < ActiveRecord::Migration[6.0]
  def change
    create_table :reservables do |t|
      t.string :name, null: false
      t.integer :price
      t.integer :parent_id, index: true

      t.timestamps
    end
  end
end

```

```shell
$ rails db:migrate
```

db/seeds.rb

```ruby
#***********************************************************
# 予約出来るもの登録   冪等性
#***********************************************************
Reservable.destroy_all
# 親のアサインの整合性を取るため
[[ActiveRecord]]::Base.connection.execute "ALTER TABLE reservables AUTO_INCREMENT = 1"  # MySQL
ActiveRecord::Base.connection.execute "delete from sqlite_sequence where name='reservables';"  # SQLite

root1 = Reservable.create(name: "会議室1")
root1.children.create(name: "椅子")
root1.children.create(name: "机")
root1.children.create(name: "プロジェクター")

root2 = Reservable.create(name: "会議室2")
root2.children.create(name: "椅子")
root2.children.create(name: "机")
root2.children.create(name: "プロジェクター")

root3 = Reservable.create(name: "多目的ホール")
root3.children.create(name: "椅子")
root3.children.create(name: "机")
root3.children.create(name: "プロジェクター")

Reservable.tree_view(:name)
```

```shell
❯ rails db:seed
root
 |_ 会議室1
 |    |_ プロジェクター
 |    |_ 机
 |    |_ 椅子
 |_ 会議室2
 |    |_ プロジェクター
 |    |_ 机
 |    |_ 椅子
 |_ 多目的ホール
 |    |_ プロジェクター
 |    |_ 机
 |    |_ 椅子
```

## 予約者・管理者・開発者

 devise で3者のモデルを管理することにする。

 admin ユーザは activeadmin のデフォルトで入れるので後回しにする。

```shell
$ rails g devise:install 
```

config/initializers/devise.rb

```ruby
config.scoped_views = true
```

```shell
$ rails g devise user
$ rails g devise manager
```

このモデルはまだないものに限るとのこと。

```shell
$ rails db:migrate
```

コントローラーとビューを生成する。名前はモデル名の複数形

```shell
$ rails g devise:controllers users
$ rails g devise:controllers managers

$ rails g devise:views users
$ rails g devise:views managers
```

ルーチングを設定

config/routes.rb

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

* * *

ActiveAdmin を入れる。

Gemfile

```ruby
gem 'activeadmin'
```

```shell
$ bundle install
$ rails g active_admin:install
$ rails db:migrate
```

db/seeds.rb

```ruby
if AdminUser.count.zero?
  puts('Add default AdminUser')
  AdminUser.create!(email: 'admin@people.co.jp', password: 'pmorioka7481')
end
```

管理リソースを登録。

```shell
$ rails g active_admin:resource manager
$ rails g active_admin:resource user
$ rails g active_admin:resource booking
```

CSS汚染対策
css と js を階層深くに移動する。

\--> assets/javascripts/admin/active_admin.js

\--> assets/stylesheets/admin/active_admin.scss

assets/stylesheets/application.css で require_tree を directory にして再帰的に探さないようにする。

```ruby
 *= require_directory .
 *= require_self
```

active_admin は active_admin の css と js のみ参照するようにする。

config/initializers/active_admin.rb

```ruby
  config.clear_stylesheets!
  config.register_stylesheet "admin/active_admin.css"
  config.clear_javascripts!
  config.register_javascript "admin/active_admin.js"
```

とりあえず開発者管理ツールはこれで。

* * *

CSS Framework を Tailwind CSS にしてみる。

その前に webpacker を install してみる。

```shell
$ rails webpacker:install
```

これで、app/javascript/packs/application.js が作られた。

[Rails 6 and Tailwind CSS via Webpacker— Getting Started](https://medium.com/@davidteren/rails-6-and-tailwindcss-getting-started-42ba59e45393)

```shell
$ mkdir app/javascript/css

$ yarn add tailwindcss
$ yarn tailwindcss init app/javascript/css/tailwind.js
```

app/javascript/css/application.css

```css
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
```

app/views/layouts/application.html.slim に1行追加した

```slim
    = csrf_meta_tags
    = csp_meta_tag
    = stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload'
    = javascript_pack_tag 'application', 'data-turbolinks-track': 'reload'
    = stylesheet_pack_tag 'application', media: 'all', 'data-turbolinks-track': 'reload'
```

app/javascript/packs/application.js に追加。

```js
import '../css/application.css'
```

postcss.config.js に追加。

```js
module.exports = {
  plugins: [
    require('postcss-import'),
    require('postcss-flexbugs-fixes'),
    // 追加
    require('tailwindcss')('./app/javascript/css/tailwind.js'),
    require('autoprefixer'),

    require('postcss-preset-env')({
      autoprefixer: {
        flexbox: 'no-2009',
      },
      stage: 3,
    }),
  ],
}

```

class名でレイアウトデザインをするタイプのフレームワーク。
これはいいな。

***

