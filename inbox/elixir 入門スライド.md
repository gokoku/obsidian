---
type: note
---

#elixir 

---
2022-07-17  00:45

# elixir 入門スライド

https://www.slideshare.net/piacere_ex/elixir1json-81100124

## はまりどころ
- バージョンが古いので、Docker でやると進む。

```
$ docker pull trenpixster/elixir
$ mkdir ~/Desktop/elixir/code
$ docker run --rm -v ~/Desktop/elixir/code:/code -i -t trenpixster/elixir /bin/bash
```

1.13.4 では、HTTPoison の依存で引っ掛かってできなかった。
Docker のは 1.4 だった。

- HTTPoison の戻り値が unmatching。

```elixir
defmodule Crawl do

  def get() do
    HTTPoison.get("https://api.github.com/repos/Elixir-lang/Elixir/issues")
    |> body
    |> Poison.decode!
    |> Enum.map( fn (%{"title" => title}) -> title end )
  end

  def body( {:ok, %{body: json_body, status_code: 200}}), do: json_body
```

body の引数を tuple にして、:ok を付けると上手くいった !!