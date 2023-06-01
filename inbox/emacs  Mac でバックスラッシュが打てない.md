#emacs

---
2022-04-15  14:17

# emacs  Mac でバックスラッシュが打てない


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/hirokisince1998/items/029741559d7ba7078523" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9SklTJUUzJTgyJUFEJUUzJTgzJUJDJUUzJTgzJTlDJUUzJTgzJUJDJUUzJTgzJTg5JUUzJTgxJUFFTWFjJUUzJTgxJUE3JUUzJTgzJTkwJUUzJTgzJTgzJUUzJTgyJUFGJUUzJTgyJUI5JUUzJTgzJUE5JUUzJTgzJTgzJUUzJTgyJUI3JUUzJTgzJUE1JUUzJTgyJTkyJUU1JTg1JUE1JUU1JThBJTlCJUUzJTgxJTk5JUUzJTgyJThCJTIwJTI4RW1hY3MlRTclOTQlQTglRTglQTglQUQlRTUlQUUlOUElRTQlQkIlOTglRTMlODElOEQlMjkmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT01NiZ0eHQtY2xpcD1lbGxpcHNpcyZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPWQ5ZDQzYWExNzNiZGRkNzQ3NzgzYjFiNzI2MWUwOTMw&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwaGlyb2tpc2luY2UxOTk4JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz02MmExMjllNmI3Y2E5YmFjYTI5MGE5MDBiNTc4ODkyZA&blend-x=142&blend-y=491&blend-mode=normal&s=ac949c91ef33e45940312ca4774c5bf8')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">JISキーボードのMacでバックスラッシュを入力する (Emacs用設定付き) - Qiita</h1>
		<p class="rich-link-card-description">
		問題の背景を理解している人は本編に飛んでください。  (追記: 投稿して満足したところで、もっとずっと簡単な方法を発見してしまった。むなしい。 ~/Library/KeyBindings/DefaultKeyBinding.dict ...
		</p>
		<p class="rich-link-href">
		https://qiita.com/hirokisince1998/items/029741559d7ba7078523
		</p>
	</div>
</a></div>


```elisp

(define-key global-map [?\M-¥] [?\\])

(defun iserch-add-backslash'()
	(interactive)
	(iserch-pringing-char ?\\ 1))
(define-key iserch-mode-map [?\M-¥] 'iserch-add-backslash)
```

