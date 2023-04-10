---
type: note
---

#elixir 

---
2022-12-08  08:37

# elixir  テストにwatchがある

https://hexdocs.pm/mix_test_watch/readme.html

```elixir
  defp deps do
    [
      {:mix_text_watch, "~> 1.0, only: :dev, runtime: false"}
    ]
  end
```

```shell
$ mix deps.get

$ mix test.watch
```

https://qiita.com/t-yamanashi/items/4dab321c12c68317cc48

test/rectangle_test.exs
```elixir
defmodule RectangleTest do
  use ExUnit.Case
  doctest Rectangle

  test_params = [
    [1,1,10,10,5,5,5,5,true],
    [  1,  1, 10, 10, 15,  5, 5, 5,false],
    [100,100, 10, 10, 90,110,10,10, true]
  ]

  for test_param <- test_params do
    @test_param test_param

    test "collided #{inspect(@test_param)}" do
      [x1,y1,w1,h1,x2,y2,w2,h2,ret] = @test_param
      assert Rectangle.collided?(x1,y1,w1,h1,x2,y2,w2,h2) == ret
    end
  end
end
```

```elixir
defmodule Rectangle do
  @doc """
  Collided detection.

  ## Examples
    iex> Rectangle.collided?(1,1,10,10,5,5,5,5)
    true
  """

  def collided?(x1, y1, w1, h1, x2, y2, w2, h2) do
    x1 <= x2 + w2 && x1 + w1 >= x2 && y1 <= y2 + h2 && y1 + h1 >= y2
  end
end
```

凄いなー
