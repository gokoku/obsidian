---
type: note
---

#ruby/rails 

---
2023-03-07  15:39

# rails 緯度経度のModelの型指定

https://easyramble.com/rails-migration-with-decimal.html

```ruby

t.decimal :latitude, precision: 11, scale: 8
t.decimal :longitude, precision: 11, scale: 8
```

全体の桁数が 11 小数点が 8 の設定。
これにしようか。

モデルに値を求めると、BigDecimalオブジェクトが返ってくるらしい。
だから Float オブジェクトにしてやるのがいいらしい。

```ruby

class Geocode < ActiveRecord::Base
  def lat
    latitude.to_f
  end
  def lon
    longitude.to_f
  end
end
```

