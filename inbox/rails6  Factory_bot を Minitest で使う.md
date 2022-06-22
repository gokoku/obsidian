#ruby/rails


## Minitest での導入

[【Rails】factory_botの使い方メモ - Qiita](https://qiita.com/at-946/items/aaff42e4a9c2e4dd58ed)

```ruby
gem 'factory_bot_rails'
```

### config/application.rb

```ruby
require 'factory_bot_rails"

  class Application < Rails::Application
    config.factory_bot false
    config.factory_bot.definition_file_paths = ['test/factories']
```

factory_bot_rails の require と、factories のディレクトリパスの指定がないと動かなかった。

require 'factory_bot_rails' が無いとうまくいかないようだ。

### test/test_helper.rb

```ruby
class ActiveSupport::TestCase
  include FactoryBot==Syntax==Methods
```

### モデルデータ生成ファイル例 test/factories/booking.rb

```ruby
FactoryBot.define do
  factory :booking do
    name "test1"
  end
end
```

### 生成の仕方

```ruby
DB登録
@booking = create(:booking)

上書きして登録
@booking = create(:booking, name: "hoge")
```

### アソシエーション

リレーションはテストのセットアップで動的に関連させるのが分かりやすいかな。

```ruby
FactoryBot.define do
  factory :root, class: Reservable do
    name { "会議室" }
  end
end
```

Reservable は自分を belong する特殊な例のモデル

```ruby
@root = create(:root)
@desk = @root.children.create(name: "机")
```
