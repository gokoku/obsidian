---
type: note
---

#ruby/rails 

---
2023-03-07  15:59

# rails 時間だけ入れたいModel

https://blog.willnet.in/entry/2015/06/12/063731

```ruby
add_column :users, :lunch_time, :time
```

:time が DB の Time 型とのこと。

ただし、
lunch_time に 12:00 が格納されてした場合は、
```ruby
user.lunch_time #=> 2000-01-01 12:00:00 UTC
```
と返ってくるとのこと。
UTC でDBに入るらしい。

Rails5 からは config/application.rb
```ruby
config.time_zone = 'Tokyo'
config.active_record.time_zone_aware_types = [:datetime, :time]
```

```ruby
user.lunch_time = '12:00'
user.lunch_time #=> Sat, 01 Jan 2000 12:00:00 JST +09:00
```
DB には UTC時間 "03:00:00" が格納されるとのことで、time型をすんなりと使えるようになったらしい。

