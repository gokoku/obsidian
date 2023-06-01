#ruby

---
2021-08-19

# Ruby RX プログラミング入門

#python RX 入門]] 

でやってた都道府県をzipコードから引くやつ。

マルチスレッドには出来なかったが、同じ動作をするやつが出来た。

```ruby
require 'rx'
require 'net/http'
require 'json'

ENDPOINT = "http://zipcloud.ibsnet.co.jp/api"
code_list = [
  "0700001",
  "7070403",
  "9051308",
]

stream = Rx::Observable.from_array code_list

stream.map(){ |x| "#{ENDPOINT}/search?zipcode=#{x}"
}.map() { |x|
  uri = URI(x)
  res = Net::HTTP.get(uri)
}.map() { |x|
  JSON.parse x
}.map(){ |x|
  x["results"][0]["address1"]
}.subscribe(
  lambda {|x| puts "都道府県 : #{x}" },   # success
  lambda {|e| p e},                       # error
  lambda { p "end" }                      # finally
)

while Thread.list.size > 1
  (Thread.list - [Thread.current]).each &:join
end
```

```
 ruby zipcode.py
都道府県 : 北海道
都道府県 : 岡山県
都道府県 : 岡山県
都道府県 : 沖縄県
"end"
```

Thread.list - [Thread.current]が謎。

こんな感じかな。
```ruby
[10] pry(main)> ([[1],[2],[3],[4]]-[[2021-05-27]]).each &:join
=> [[2], [3], [4]]
```

