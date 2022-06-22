#docker #elixir 

---
2022-02-07  21:54

# docker  Elixir の環境

Elixir 実践ガイド本の Docker 環境。

```shell
ex_book1
├── Dockerfile
└── docker-compose.yml
```

```docker:Dockerfile
FROM elixir:1.11.2

ARG UID=1000
ARG GID=1000
RUN groupadd -g $GID devel
RUN useradd -u $UID -g $GID -m devel

COPY --chown=devel:devel . /ex_book1
USER devel
WORKDIR /ex_book1
RUN mix local.hex --force
```

```yaml:docker-compose.yml
version: "3"
services:
ex_book1:
build: .
volumes:
- .:/ex_book1
working_dir: /ex_book1
```

```shell
$ docker-compose build ex_book1

$ docker-compose run --rm ex_book1 /bin/bash
devel@c40d14766d7e:/ex_book1$ ls -la

Dokerfile
docker-compose.yml
```

bash 終了時に ex_book1 が削除される。
ワークスペースが同じディレクトリになってる。

## コンパイル

#### バイトコード

```shell
$ mkdir ch03
$ cd ch03
```

ch03/greeting.ex

```elixier:ch03/greeting.ex
defmodule Greeting do
  def hello(name) do
    IO.puts("Hello, #{name}!")
  end
end
```

```shell
$ elixirc greeting.ex
```

Elixir.Greeting.beam が生成された。これがバイトコード。

実行。

```shell
$ elixir -e 'Greeting.hello("world")'
Hello, world!
```


#### スクリプト

```elixir:ch03/hello.exs
name = IO.gets("What is your name? ")
name = String.trim(name)
Greeting.hello(name)
```

```shell
$ elixir hello.exs
What is your name? george
Hello, george!
```

同じディレクトリ階層のバイトコードを読み込んでいるのがわかる。

#### シバン

```shell:ch03/hello
#!/usr/local/bin/elixir

name = IO.gets("What is your name? ")
name = String.trim(name)
Greeting.hello(name)
```

```shell
$ chmod u+x hello
$ ./hello
What is your name? george
Hello, george!
```

#### IEx
```shell
$ iex
> Greeting.hello("George")
> Hello, George!
> :ok
```

