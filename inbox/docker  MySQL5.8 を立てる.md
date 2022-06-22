---
type: note
---

#docker

---
2022-05-11  13:26

# docker  MySQL5.8 を立てる
```shell
├── README.md
├── docker
│   └── mysql
│       ├── Dockerfile
│       └── conf.d
│           └── my.cnf
└── docker-compose.yml
```

docker-compose.yml

```yaml
version: '3.3'
services:
  db:
    build: ./docker/mysql
    #image: mysql:5.8
    #restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
    ports:
      - "3306:3306"
    volumes:
      - ./docker/mysql/initdb.d:/docker-entrypoint-initdb.d
      - ./docker/mysql/conf.d:/etc/mysql/conf.d
      - ./var/log/mysql:/var/log/mysql
      - ./var/lib/mysql:/var/lib/mysql
```

docker/mysql/Dockerfile

```Dockerfile
FROM mysql:8.0
RUN mkdir /var/log/mysql && touch /var/log/mysql/mysqld.log # 指定の場所にログを記録するファイルを作る
```

docker/mysql/conf.d/my.cnf

```config
[mysqld]
#character-set-server=utf8mb4       # mysqlサーバー側が使用する文字コード
explicit-defaults-for-timestamp=1　  # テーブルにTimeStamp型のカラムをもつ場合、推奨
general-log=1　                  # 実行したクエリの全ての履歴が記録される（defaultではOFFになっているらしい）
general-log-file=/var/log/mysql/mysqld.log # ログの出力先
[client]
#default-character-set=utf8mb4　　　　　　　　　　　　　　　# mysqlのクライアント側が使用する文字コード
```

## 起動

```shell
$ git clone git@gitlab.alfa-people.ml:docker/mysql_8.git
$ cd mysql_8
$ docker compose up -d
$ mysql -h 127.0.0.1 -u root -p
Enter password: root
mysql> select version();
+-----------+
| version() |
+-----------+
| 8.0.29    |
+-----------+
1 row in set (0.00 sec)
```


### バージョンの確認
```shell
$ mysql -h 127.0.0.1 -uroot -p

mysql> select version();
+-----------+
| version() |
+-----------+
| 8.0.29    |
+-----------+
1 row in set (0.00 sec)
```

8である。


### コンテナの中に入りたい
すでに docker compose up して起動してるコンテナに入りたい。

```shell
$ docker ps
caf236.................mysql_58-db-1

$ docker exec -it mysql_58-db-1 /bin/bash
```