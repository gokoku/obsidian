---
type: note
---

#postgresql 

---
2023-01-13  09:38

# postgres  ソケットError

```
$ createuser postgres -s
createuser: error: connection to server on socket "/tmp/.s.PGSQL.5432" failed: No such file or directory
	Is the server running locally and accepting connections on that socket?
```

無ししても無駄。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://zenn.dev/yusuke99/scraps/c4fa33411de09f" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://storage.googleapis.com/zenn-user-upload/avatar/f7d619e53f.jpeg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">【PostgreSQL】Connection to server on socket "/tmp/.s.PGSQL.5432" failed: No such file or directory</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://zenn.dev/yusuke99/scraps/c4fa33411de09f
		</p>
	</div>
</a></div>



/usr/local/var/log/postgres@14.log

```
2023-01-13 09:40:36.384 JST [22451] FATAL:  lock file "postmaster.pid" already exists
2023-01-13 09:40:36.384 JST [22451] HINT:  Is another postmaster (PID 571) running in data directory "/usr/local/var/postgresql@14"?
```

lock file があると言ってる。

```shell
$ cd /usr/local/var
$ rm -rf postgresql@14
```

再インストール。
```shell
$ brew reinstall postgresql

$ psql
error でも
$ createdb 'whoami'

```
