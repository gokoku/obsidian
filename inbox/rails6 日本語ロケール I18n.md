#ruby/rails 



[【Ruby on Rails】Railsの日本語化対応 - Qiita](https://qiita.com/tkr_ld/items/d010d3f8c6cce5e14f58)

config/initializers/locale.rb を作る。

```ruby
Rails.application.config.i18n.default_locale = :ja
```

日本語 ja.yml をダウンロード。

```shell
$ wget https://raw.githubusercontent.com/svenfuchs/rails-i18n/master/rails/locale/ja.yml
```

これを config/locales に設置する。

devise用

```ruby
gem 'devise-i18n'
gem 'devise-i18n-views'
```

```shell
$ rails g divese:views:locale ja
```

config/locales/devise.views.ja.yml が生成される。
