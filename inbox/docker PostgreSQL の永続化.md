---
type: note
---

#docker #postgresql 

---
2022-10-12  11:10

# docker PostgreSQL の永続化

```shell
postgres_14
├── docker
│   └── postgres
└── docker-compose.yml
```

docker-compose.yml

```yaml
version: '3'

services:
  db:
    image: postgres:14
    container_name: postgres
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - type: bind
        source: ./docker/postgres
        target: /var/lib/postgresql/data
```

## type: bind
https://zenn.dev/sarisia/articles/0c1db052d09921

1行で書けるやり方の他に long syntax があるとのこと。
ロングにすると、ディレクトリがないとエラーになって止まるのでデバッグがしやすいとのこと。

short syntax だと、docker が root 権限で掘っちゃうので出来るけど、使えない状態になってしまうとのこと。

これに引っ掛かってたのか!!!
権限が無いとエラーで docker が動かない現象は、docker が自分で掘ってディレクトリ作ってしまうことで、george ユーザーで compose up すると、権限がないと動かなかった原因。
自分で mkdir してたら問題なかったが、まんま compose up したから、次が動かなくなったのか。