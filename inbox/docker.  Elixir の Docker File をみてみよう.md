---
type: note
---

#docker

---
2022-06-02  21:04

# docker.  Elixir の Docker File をみてみよう
```
ex_book1 　┯─ Dockerfile
           ┗゜docker-compose.yml
```

### Dockerfile
http://docs.docker.jp/index.html

```dockerfile
FROM elixir:1.11.1

ARG UID=1000
ARG GID=1000
RUN groupadd -g $GID devel
RUN useradd -u $UID -g $GID -m devel
RUN apt-get update && apt-get install -y emacs

COPY --chown=devel:devel . /ex_book1
USER devel
WORKDIR /ex_book1
RUN mix local.hex --force
```

FROM : ペースのDockerイメージ名
ARG : 変数(=1000 はデフォルト値)　--build-arg でユーザーが指定できる
COPY : --chown=devel:devel は権限と一緒にコピー.   コピー元(ホスト) --> イメージのコピー先
WORKDIR : ホストの作業ディレクトリか? それともイメージの方だろうか。

どうも emacs が無いと不便だ。なので、追加する。

## docker-compose.yml

https://docs.docker.jp/compose/overview.html

```yaml
version: "3"
services:
  ex_book1:
    build: .
    volumes:
      - .:ex_book1
    working_dir: /ex_book1
```

build : Dockerfile のあるところ?
volumes :   マウントしたいホストのパス : ターゲット (: モード ro read only,  rw)


```shell
$ docker compose build ex_book1


$ docker compose run --rm ex_book1 /bin/bash
```

--rm : コンテナ実行後に削除する
run サービス名 となる



