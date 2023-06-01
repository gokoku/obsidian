#elixir 

---
2022-03-11  13:35

# elixir   Phoenix 入門
## PostgresSQL の起動
記事を書いた人のイメージで動かす。
```shell
$ docker run -d --rm -p 5432:5432 -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres postgres:13
```

## Phoenix install
```shell
$ mix local.hex
$ mix archive.install hex phx_new
$ mix phx.new --version
Phoenix installer v1.6.6
```

Hex は Elixir, Erlang 用のパッケージマネージャ

Hex は mix コマンドを使ってインストールとか行う。

phx.new ジェネレータで Phoenix プロジェクトを作成する。

```shell
$ mix phx.new realworld
$ cd 

$ git init
$ git add .
$ git commit -m 'first commit.'
```
## データベースの初期設定
```shell
$ mix ecto.setup
```

## Phoenix の起動

```shell
$ mix phx.server
```
![[Pasted image 20220311142501.png | 500]]

