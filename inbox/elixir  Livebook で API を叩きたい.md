---
type: note
---

#elixir 

---
2023-01-18  22:05

# elixir  Livebook で API を叩きたい

## HTTPoison

setup
```elixir
Mix.install([:httpoison, :jason])
```


```elixir
"https://zipcloud.ibsnet.co.jp/api/search?zipcode=0200611"
|> HTTPoison.get!()
|> Map.get(:body)
|> Jason.decode!()
```

```
%{
  "message" => nil,
  "results" => [
    %{
      "address1" => "岩手県",
      "address2" => "滝沢市",
      "address3" => "巣子",
      "kana1" => "ｲﾜﾃｹﾝ",
      "kana2" => "ﾀｷｻﾞﾜｼ",
      "kana3" => "ｽｺﾞ",
      "prefcode" => "3",
      "zipcode" => "0200611"
    }
  ],
  "status" => 200
}
```

## req 

```
Mix.install([:httpoison, :jason, {:req, "~> 0.2.1"}])
```
```elixir
"https://qiita.com/api/v2/items?query=tag:Elixir"
|> Req.get!()
|> Map.get(:body)
|> Enum.map(& Map.take(&1, ["title", "url"]))
```


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/torifukukaiou/items/4d842c6acae2b8967467" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9UmVxJUUzJTgyJTkyJUU0JUJEJUJGJUUzJTgxJUEzJUUzJTgxJUE2JUUzJTgxJUJGJUUzJTgyJThCJTIwJTI4RWxpeGlyJTI5JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz00NTJlMzY2ODJlYmE1NzM4MjUyNmU2MTBhOWFhYzMzZg&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwdG9yaWZ1a3VrYWlvdSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9MjEyNDg5ZjlhMjgyYWI5MzQwYzgxZTU2MTczMjViOTY&blend-x=142&blend-y=491&blend-mode=normal&s=ae361b4f070cd1e191b2be4422fb3511')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Reqを使ってみる (Elixir) - Qiita</h1>
		<p class="rich-link-card-description">
		昔より 主を内海の 野間なれば 報いを待てや 羽柴筑前  Advent Calendar 2022 13日目1の記事です。 I'm ready for 12/25,2022  I'm looking forward to  12/...
		</p>
		<p class="rich-link-href">
		https://qiita.com/torifukukaiou/items/4d842c6acae2b8967467
		</p>
	</div>
</a></div>

https://httpbin.org で Post を試せるらしい。

```
$ curl -X POST "https://httpbin.org/post" -H "accept: application/json"
```

これをやってみよう。

```elixir
"https://httpbin.org/post"
|> Req.post!("accept: application/json")
|> Map.get(:body)
|> Map.get("data")
```
"accept: application/json"

https://reqres.in/

ここもテストに使える。


```

POST https://reqres.in/api/users HTTP/1.1
Content-Type: application/json

{
  "name": "morpheus",
  "job": "leader"
}
```


これをやってみる。

```elixir
"https://reqres.in/api/users"
|> Req.post!({:json, %{'name'=>'morpheus', 'job'=> 'leader'}})
```

```elixir
headers = [
	{"Content-Type", "application/json"}
]
json_data = %{'name'=>'morpheus', 'job'=>'leader'} |> Jason.encode!()

"https://reqres.in/api/users"
|> Req.post!({:json,%{'name'=>'morpheus', 'job'=>'leader'}}, headers)
```

```shell
$ curl -i -H 'Content-Type: application/json' \
-d '{ "name": "morpheus", "job": "leader"}' \
https://reqres.in/api/users
```

