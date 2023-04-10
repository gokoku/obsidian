---
type: note
---

#

---
2022-12-08  08:59

# Elixir School. Mix

https://elixirschool.com/ja/lessons/basics/mix

```shell
$ mix new example
$ cd example
$ iex -S mix
```

```shell
$ mix compile
```

パッケージのインストール
```elixir
def deps do
  [
	{:phoenix, "~> 1.1 or ~> 1.2"}]
    {:phoenix_html, "~> 2.3"},
    {:cowboy, "~> 1.0", only: [:dev, :test]},
    {:slime, "~> 0.14"}
  ]
```

```shell
$ mix deps.get
```

## 環境

```shell
$ MIX_ENV=prod mix compile


$ iex -S mix
> Mix.env
:dev
```

prod : production
dev : development
test : test

か
