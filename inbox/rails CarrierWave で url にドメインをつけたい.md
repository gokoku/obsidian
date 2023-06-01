---
type: note
---

#ruby/rails 

---
2023-04-19  15:16

# rails CarrierWave で url にドメインをつけたい


config/environments/production.rb
```ruby
  config.asset_host = "https://yahaba-signage.pmorioka.com"
```


config/initializers/carrier_wave.rb
```ruby
CarrierWave.configure do |config|
    config.storage = :file
    config.asset_host = ActionController::Base.asset_host
end
```

例えば、Post モデルに image でアップロードしたい。

```ruby
class Post < ApplicationRecord
  mount_uploader :image, PostImageUploader
end
```

```shell
$ rails c -e production

> Post.first.image.url
"https://example.com/upload/....../example.png"
```

OK!!

asset_host は asset:precompile で生成される URL に使われる。
　　　　　　　　　　　　　　　　image javascript css 

