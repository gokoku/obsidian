---
type: note
---

#elixir 

---
2023-02-21  15:23

# elixir タプルの集約 reduce

```elixir
a =[
  {1, 10},
  {2, 2},
  {3, 3},
  {4, 4},
  {5, 5}
]

a
|> Enum.reduce(fn {x, y}, {acc1, acc2} ->
  {x + acc1, y + acc2}
end)
```

座標のタプルの平均を取りたいので、どうにかならないかなというモチベーション。
からの平均値。

```elixir
a =[
  {1, 10},
  {2, 2},
  {3, 3},
  {4, 4},
  {5, 5}
]

count = a |> Enum.count()
a
|> Enum.reduce(fn {x, y}, {acc1, acc2} ->
    {x + acc1, y + acc2}
  end)
|> then(& {elem(&1,0) / count, elem(&1,1) / countfn} )
```
{7.5, 12.0}
いいんじゃね?


