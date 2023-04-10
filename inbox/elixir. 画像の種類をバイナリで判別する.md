---
type: note
---

#elixir 

---
2022-12-08  16:45

# elixir. 画像の種類をバイナリで判別する




<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/the_haigo/items/68f52a6f9623d2da7d26" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9RWxpeGlyJUUzJTgxJUFFJUUzJTgzJTkwJUUzJTgyJUE0JUUzJTgzJThBJUUzJTgzJUFBJUUzJTgzJTg3JUUzJTgzJUJDJUUzJTgyJUJGJUUzJTgzJTkxJUUzJTgyJUJGJUUzJTgzJUJDJUUzJTgzJUIzJUUzJTgzJTlFJUUzJTgzJTgzJUUzJTgzJTgxJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz01M2JhMTE5ZGE0NWZhOTY3YzRkMzNhNzM5Y2ZmYjMzZQ&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwdGhlX2hhaWdvJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz0zNzE4MDQxNmZiNjNkOGNiYmI5ODNkNmUxMjczMDVjMw&blend-x=142&blend-y=491&blend-mode=normal&s=e561557b964d62df78588539d960631a')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Elixirのバイナリデータパターンマッチ - Qiita</h1>
		<p class="rich-link-card-description">
		この記事は「Elixir Advent Calendar 2022その1」2日目の記事です．  はじめに 本記事は以下のElixir特集のパターンマッチの章でページ数の関係上削られたので、こちらで供養します バイナリデータのパター...
		</p>
		<p class="rich-link-href">
		https://qiita.com/the_haigo/items/68f52a6f9623d2da7d26
		</p>
	</div>
</a></div>


```elixir
iex(1)> defmodule WebDB.Binary do
...(1)>   def mime(<<0x89, "PNG", _::bits>>), do: "png"
...(1)>   def mime(<<"GIF", 0x38, _::bits>>), do: "gif"
...(1)>   def mime(<<0xFF, 0xD8, _::bits>>), do: "jpg"
...(1)>   def mime(_), do: "unsupported file"
...(1)> end


> {:ok, file} = File.read("self.png")
> WebDB.Binary.mime(file)
> "png"
```

すげー!! バイナリデータの解析が C より簡単に出来そうな気がする。

