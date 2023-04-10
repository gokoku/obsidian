---
type: note
---

#pop_os  #postgresql 

---
2022-12-02  22:08

# pop_os  PostgreSQL をいれる


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/piacerex/items/01e89435af0f7a454ad2" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9VWJ1bnR1JTIwMjIuMDQlRTMlODElQTdFbGl4aXIlRUYlQkMlOEJOeCVFRiVCQyU4RlBob2VuaXglRUYlQkMlOEJQb3N0Z3JlU1FMJUUzJTgxJUFFJUU2JTlDJTgwJUU3JTlGJUFEJUU2JTg5JThCJUU5JUEwJTg2JUVGJUJDJTg4RWxpeGlyJUU2JTlDJTgwJUU2JTk2JUIwJUU3JTg5JTg4JUUzJTgxJUI4JUUzJTgxJUFFJUUzJTgyJUEyJUUzJTgzJTgzJUUzJTgzJTk3JUUzJTgyJUIwJUUzJTgzJUFDJUUzJTgzJUJDJUUzJTgzJTg5JUUzJTgwJTgxV1NMMiVFRiVCQyU4QlVidW50dSUyMDIyLjA0JUUzJTgxJUFFJUUyJTgwJUE2JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz0yYmJmZGQwODY4MTlhMTFlM2FhZDNhNTA0OWRhNmNmMw&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwcGlhY2VyZXgmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPWNmNGVlYzVlMTc2ZDA1MTdiNjA1N2FlOGYyYjA5YmQ1&blend-x=142&blend-y=491&blend-mode=normal&s=cfef47ce1935c50e9238be6c703b6d5e')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Ubuntu 22.04でElixir＋Nx／Phoenix＋PostgreSQLの最短手順（Elixir最新版へのアップグレード、WSL2＋Ubuntu 22.04のオマケ付き） - Qiita</h1>
		<p class="rich-link-card-description">
		この記事は、Elixir Advent Calendar 2022 10の2日目です 昨日は、 @RyoWakabayashi さんで「Elixir Explorer 0.4.0 で追加された Query を使うと、データ分析が更に捗...
		</p>
		<p class="rich-link-href">
		https://qiita.com/piacerex/items/01e89435af0f7a454ad2
		</p>
	</div>
</a></div>



```
$ sudo apt install curl ca-certificates gnupg -y
$ curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
$ sudo apt update
$ sudo apt install postgresql -y
$ sudo service postgresql start
$ sudo su - postgres
$ psql
$ postgres=# alter role postgres with password 'postgres';
$ postgres=# \q
$ exit
```


