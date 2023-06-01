#elixir 

### プログラミング Elixir の練習問題

こうじゃないんだろうなー。
でも、なんとか出来たようなので。

```ruby
defmodule Chop do

  def guess(actual, range) do
    min..max = range
    arv = _arv(range)
    cond do
      actual > arv ->
        guess(actual, arv..max)
      actual < arv ->
        guess(actual, min..arv)
      true ->
        actual
    end
  end

  def _arv(range) do
    min..max = range
    arv = div(max - min, 2) + min
    IO.puts("Is it #{arv}")
    arv
  end
end
```

```shell
iex> Chop.guess(273, 1..1000)
Is it 500
Is it 250
Is it 375
Is it 312
Is it 281
Is it 265
Is it 273
273
```
## 多分こうだ!!

ガード節を使って同じ関数を分けると if が無くなる。
ガード節自体が条件分岐を担うのか。


```ruby
defmodule Chop do
  def guess(actual, range) do
    min..max = range
    arv = div(max + min, 2)
    _guess(actual, avr, min, max)
  end
  
  defp _guess(actual, avr, _, max) when avr < actual do
    IO.puts("Is it #{arv}")
    min = avr
    avr = div(max + min, 2)
    _guess(actual, avr, min, max)
  end
  
  defp _guess(actual, avr, min, _) when avr > actual do
    IO.puts("Is it #{arv}")
    max = avr
    avr = div(max + min, 2)
    _guess(actual, avr, min, max)
  end
  
  defp _guess(actual, avr, _, _) when actual == avr do
    IO.puts(actual)
  end
end
```