---
type: note
---

#db #mysql 

---
2022-04-28  14:57

# Sequel Ace の ssh_config

<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/makoll/items/d462c1c2814083b3ae6b" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9U2VxdWVsJTIwQWNlJUUzJTgxJUE3JUU4JUI4JThGJUUzJTgxJUJGJUU1JThGJUIwJUU3JUI1JThDJUU3JTk0JUIxJUUzJTgxJUE3JUU2JThFJUE1JUU3JUI2JTlBJUUzJTgxJTk5JUUzJTgyJThCJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz04MjBlMGUzNmQ1NTEwODMwOWJhNzdmNDUzZWUzNGE4OA&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwbWFrb2xsJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz1iNzZlZDAwY2I5ZWQzZDkxNTY0MjA3NjYxMWQ1NDU4Yg&blend-x=142&blend-y=491&blend-mode=normal&s=c8b91f354a6a9a5c8cbfe9c5e2522697')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Sequel Aceで踏み台経由で接続する - Qiita</h1>
		<p class="rich-link-card-description">
		最初に  Sequel AceはSequel Proの後継ということで、 SSHを利用しての接続機能を持っており、つまり踏み台経由での接続も可能です しかし、仕様によりProと同じように設定しても接続できなかったので注意が必要です...
		</p>
		<p class="rich-link-href">
		https://qiita.com/makoll/items/d462c1c2814083b3ae6b
		</p>
	</div>
</a></div>

### Proxy を介さない SSH トンネル接続

Preferences に設定するといいらしい。

![[Pasted image 20220428153901.png | 400]]
Add Files に **.ssh/config** と **.ssh/known_hosts** を追加するとのこと。

![[Pasted image 20220428154205.png | 500]]

ここで設定することで、

![[Pasted image 20220428151411.png | 300]]

成功した!!

![[Pasted image 20220428154258.png]]

Sequel Ace は Sandbox Mode で実行されるので、~/.ssh/config にアクセスできないかららしい。

---
### corkscrew を介したSSHトンネル接続が失敗する

結論、connect を使って繋がる。

[[ssh プロキシクライアントを brew の connect にしてみる]]

これで、Sequel Ace にトライしてみる。
ダメだった。

```
env: /usr/local/bin/connect: Operation not permitted
```

これかな。
