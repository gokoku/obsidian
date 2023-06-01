#ruby

---
2022-03-09  19:46

# ruby Ruby 3.1 バターンマッチの例

```ruby
require 'json'

text = '{"name": "ko1", "age": 20}'
h = JSON.parse(text, symbolize_names: true)

puts "Hello #{h[:name]} (#{h[:age]})"
```

Hello ko1 (20)

