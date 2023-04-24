---
type: note
---

#ruby/rails 

---
2023-04-24  18:08

# rails データベースを元に戻したい


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/programming_owl/items/25fbba4423c94251f56f" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9JUUzJTgwJTkwUmFpbHMlRTMlODAlOTElRTMlODMlODclRTMlODMlQkMlRTMlODIlQkYlRTMlODMlOTklRTMlODMlQkMlRTMlODIlQjklRTMlODIlOTIlRTUlODUlODMlRTMlODElQUIlRTYlODglQkIlRTMlODElOTklRTYlOTYlQjklRTYlQjMlOTUmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT01NiZ0eHQtY2xpcD1lbGxpcHNpcyZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTU3Y2EyMjk3MWRlZTI1MWQyODkxNmQ2OWFjMDlmNjY2&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwcHJvZ3JhbW1pbmdfb3dsJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz1kMGNhNzMyZTA0NDQ2YTU5YzA0OTJlNjI4MjMwZTdhMA&blend-x=142&blend-y=491&blend-mode=normal&s=7d50eb6d648fa4091a78a2d45e58283d')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">【Rails】データベースを元に戻す方法 - Qiita</h1>
		<p class="rich-link-card-description">
		はじめに Railsでアプリを開発しているときに、、 やっばっ！！間違って「rails db:migrate」しちゃった！！ ってときありますよね。 そんな時に使える「元に戻す方法」を伝授します  元に戻す方法  マイグレーションの...
		</p>
		<p class="rich-link-href">
		https://qiita.com/programming_owl/items/25fbba4423c94251f56f
		</p>
	</div>
</a></div>

sqlite3 から mysql にしたい。
なのでまるっきり元に戻してからのマイグレーションがしたい。

```shell
$ rails db:migrate VERSION=0
```

