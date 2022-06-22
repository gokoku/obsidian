#ruby

---
2022-03-09  20:28

#  3.1 はDebug が一新したらしい

```ruby
require 'debug'

3.times do
  puts "Hello"
  debugger
end
```

byebug での性能劣化がないとのこと。

rdbg コマンドを使うとプログラム開始と共に REPL が開くので、ここで　break point
を指定したりとか

```ruby:debug.rb

def fib n
  return n if n < 2
  fib(n-2) + fib(n-1)
end

fib(10)
```

```shell
$ rdbg debug.rb

[1, 6] in debug.rb
=>   1| def fib n
     2|   return n if n < 2
     3|   fib(n-2) + fib(n-1)
     4| end
     5|
     6| puts fib(10)
=>#0	<main> at debug.rb:1
(rdbg)
```

デバッガだ。どうやっていじるのだろう。

## VScode に vscode-rdbg 拡張
![[Pasted image 20220309204732.png]]

