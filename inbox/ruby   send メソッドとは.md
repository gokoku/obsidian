#ruby 


オブジェクトのメソッドを name で呼び出して実行するとのこと。

Object#send( name, [arg,...] )

シンボルで呼ぶのが良さげ？
文字列でもOK

```ruby
"19".send(:to_i, 16)
>25
```

は

```ruby
"19".to_i(16)
>25
```

と一緒だ。

