---
type: note
---

#elixir #pop_os 

---
2022-10-11  15:30

# elixir  POP-OS の Docker の Postgresql で Phoenix を動かす

POP-OS で Docker の Postgresql を動かせた。

POP-OS で 
```shell
$ cd docker/postgres_13
$ docker compose up
```


クライアントマシンで SSH ポートフォワーディングする。

```shell
$ ssh -NL 127.0.0.1:5432:127.0.0.1:5432 pop
```

pop は .ssh/config で設定している。
```config
Host pop
  Hostname pop-os.local
  User george
```

これで、iMac であたかも立ち上がっているかの如く PostgreSQL が動いてるので、Ecto セットアップもマイグレーションもバッチリデフォルトで動いた。

cui でも入れる。

```shell
$ psql postgres -U postgres -h localhost

postgres=# \l
```


imac27 に直に入れた。

[[pop_os  PostgreSQL をいれる]]


