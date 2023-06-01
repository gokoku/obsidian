#ruby

---
2022-03-09  20:10

# ruby  ブロックの入門

ブロックは無名関数っぽいやつ?

作り方は Proc を使う。

```ruby
proc = Proc.new{|x| puts x + 1 }

proc.call(2)  #=> 3
```

```ruby

def foo(x, &blk)
	blk.call(x)
end

foo(2, &proc) #=> 3
```

