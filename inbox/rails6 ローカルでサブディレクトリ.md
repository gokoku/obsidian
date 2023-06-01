#ruby/rails 



[Ruby on Railsのアプリをサブディレクトリで動かす方法 | 株式会社ランチェスター](https://www.lanches.co.jp/blog/3518)

RAILS_RELATIVE_URL_ROOT 環境変数をローカルサーバで受け取って動作させる。

config.ru

```ruby
require_relative 'config/environment'
map Rails.application.config.relative_url_root || "/" do
  run Rails.application
end

```

サーバ起動。

```shell
RAILS_RELATIVE_URL_ROOT=/demo bin/rails s
```

これで localhost:3000/demo/ で動作するようになる。

* * *

## request.referrer を参照するとき

```ruby
  def previous_controller
    url = Rails.application.routes.recognize_path(request.referrer)
    url[:controller]
  end
```

直接リクエストURLから判断するには、RAILS_RELATIVE_URL_ROOT を外さないとエラーになる。

```ruby
  def previous_controller
    url = Rails.application.routes.recognize_path(referrer_url(request.referrer))
    url[:controller]
  end

  def referrer_url(url)
    # サブディレクトリが合った場合に対応
    unless Rails.application.config.relative_url_root.blank?
      url.gsub(Rails.application.config.relative_url_root, '')
    end
  end
```

RAILS_RELATIVE_URL_ROOT が設定されてないときは、引数が評価されたままのようなので、これでOK。
