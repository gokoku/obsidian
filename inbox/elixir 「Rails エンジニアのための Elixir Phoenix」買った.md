#elixir

---
2021-07-15

# Rails エンジニアのための Elixir Phoenix 電子書籍

https://techbookfest.org/mypage/bookshelf

![[Pasted image 20210715093732.png]]

アカウントは専用で作ってある。

## 事始め

ここが基礎でまとまって学べるらしい。

https://elixirschool.com/ja/lessons/basics/basics/

インストール。
```shell
$ asdf plugin add elixir
$ asdf plugin add erlang
$ asdf plugin add nodejs
```

後はそれぞれバージョンを install する。

```shell
$ asdf install elixir 1.10.4
$ asdf install erlang 23.3.1  // ここでやっと 23 が入った
```

Hex と phx_new をインストールする。

```shell
$ mix archive.install hex phx_new 1.5.4
$ mix phx.new .
```

データベース install
```shell
$ mix ecto.create

[error] GenServer [[PID]]<0.3975.0> terminating
Last message: nil
State: Postgrex.Protocol
```
失敗する。postgress がないから。そして、ユーザーpostgres がいなくて、Superuser 出ないことが原因だった。

#postgresql  インストール]]

```shell
$ brew install postgresql

$ createuser postgres -s


もう一度
$ mix ecto.create # OK!!!
```


Phoenix 起動。

```shell
$ mix phx.server
```

![[Pasted image 20210715115308.png]]

## CRUD が出来る

```shell
$ mix phx.gen.html Posts Post posts body:text

$ mix ecto.migrate

$ mix phx.server
```

https://localhost:4000/posts

![[Pasted image 20210715135150.png]]

スキャフォールドだ。

Postgresql がデフォルトなのか。

https://qiita.com/yyh-gl/items/6b4d07c256d1bf3e7eb8

指定してやれば MySQL でいける。db/config.yml にあたるファイル  config/dev.exs で指定するとOK。

```shell
$ mix phx.new example --no-brunch --database mysql
```

