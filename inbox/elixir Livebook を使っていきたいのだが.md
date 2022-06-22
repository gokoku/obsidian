#elixir 

---
2022-01-28  17:48

# elixir Livebook を使っていきたいのだが

[[elixir Livebook というものがある]]

起動する。

```shell
$ MIX_ENV=prod mix phx.server
```
url をブラウザに貼る。

![[Pasted image 20220128180512.png]]

Markdown での mermaid でグラフ書き。
v0.5の目玉らしい。

通り過ぎると図だけになるが、ダブルクリックでコードが現れる。



実は Obsidian でもグラフになる。

```mermaid
graph TD;
  A-->B;
  A-->C;
  B-->D;
  C-->D;
```

![[Pasted image 20220128181430.png]]

エンティティ図はよくわかってないな。

![[Pasted image 20220128181816.png]]

サクサクと Web API が取れる。
```elixir
Mix.install [{:req, "~> 0.2.1"}]

"https://qiita.com/api/v2/items?query=tag:Elixir"
|> URI.encode()
|> Req.get!(finch_options: [pool_timeout: 50000, receive_timeout: 50000])
|> Map.get(:body)
|> Enum.map(& Map.take(&1, ["title", "url"]))
```

すげーよ。
