#ruby

---
2021-11-29

```ruby
class Sample 
  def a(s)
    puts "a" + s
  end
  def b(s)
    puts "b" + s
  end
end

sample = Sample.new

[:a, :b].each {|f| sample.send(v, ":hello")}
a:hello
b:hello

```
