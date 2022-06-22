#elixir 

---
2022-02-22  21:43

# elixir  mix_install_examplesからbenchee.exsの紹介です（Elixir）

https://qiita.com/torifukukaiou/items/2a2acad0cd07ec5cd9ff
<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/torifukukaiou/items/2a2acad0cd07ec5cd9ff" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9bWl4X2luc3RhbGxfZXhhbXBsZXMlRTMlODElOEIlRTMlODIlODliZW5jaGVlLmV4cyVFMyU4MSVBRSVFNyVCNCVCOSVFNCVCQiU4QiVFMyU4MSVBNyVFMyU4MSU5OSVFRiVCQyU4OEVsaXhpciVFRiVCQyU4OSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9Yzc0OWU5ZDNiYTk3Yzc2MDg1YzlmZWE2NmUyMjYzNjc&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwdG9yaWZ1a3VrYWlvdSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9MjEyNDg5ZjlhMjgyYWI5MzQwYzgxZTU2MTczMjViOTY&blend-x=142&blend-y=491&blend-mode=normal&s=009cadbccf222849002a9e31468da00b')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">mix_install_examplesからbenchee.exsの紹介です（Elixir） - Qiita</h1>
		<p class="rich-link-card-description">
		ものが売れないのは高いか悪いのかのどちらかだ Advent Calendar 2022 40日目1の記事です。 I'm looking forward to 12/25,2022  私のAdvent Calendar 2022 一覧。...
		</p>
		<p class="rich-link-href">
		https://qiita.com/torifukukaiou/items/2a2acad0cd07ec5cd9ff
		</p>
	</div>
</a></div>


```elixir:benchee.exs

Mix.install([
  {:benchee, "~> 1.0"}
])

list = Enum.to_list(1..10_000)
map_fun = fn i -> [i, i * i] end

Benchee.run(
  %{
    "flat_map" => fn -> Enum.flat_map(list, map_fun) end,
    "map.flatten" => fn -> list |> Enum.map(map_fun) |> List.flatten() end
  },
  time: 10,
  memory_time: 2
)
```


```shell
$ elixir benchee.exs
```

```elixir:qiita.exs
Mix.install [{:req, "~> 0.2.1"}]

"https://qiita.com/api/v2/items?query=tag:Elixir"
|> URI.encode()
|> Req.get!(finch_options: [pool_timeout: 50000, receive_timeout: 50000])
|> Map.get(:body)
|> Enum.map(& Map.take(&1, ["title", "url"]))
|> IO.inspect
```

```shell
$ elixir qiita.exs

```

