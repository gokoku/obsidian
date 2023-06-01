#ruby/rails 


[ruby on rails - TimeWithZone & Time.zone.now integration test fails - Stack Overflow](https://stackoverflow.com/questions/30604110/timewithzone-time-zone-now-integration-test-fails)

同じものを入れてるのに変換で微差が生じるらしいので、吸収する assert を使う。

```ruby
assert_in_delta @begin_time,  @booking.vartual_attributes( "begin_time" ), 1.second

```

1.second 以内ならオッケーとしうことで。
