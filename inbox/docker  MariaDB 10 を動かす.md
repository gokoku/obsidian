---
type: note
---

#docker #mariadb

---
2022-05-11  15:18

# docker  MariaDB 10 を動かす

```shell
├── README.md
├── docker
│   └── mysql
│       ├── Dockerfile
│       └── conf.d
│           └── my.cnf
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

```shell
FROM mariadb:10
RUN touch /var/log/mysql/mysqld.log # 指定の場所にログを記録するファイルを作る
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

## 起動する

```shell
$ git clone git@gitlab.alfa-people.ml:docker/mariadb_10.git
$ cd mariadb_10
$ docker compose up -d
$ mysql -h 127.0.0.1 -u root -p

$ mysql -h 127.0.0.1 -P 3309 -uroot -p # ポート指定の場合

Enter password: root
mysql> select version();
+-------------------------------------+
| version()                           |
+-------------------------------------+
| 10.7.3-MariaDB-1:10.7.3+maria~focal |
+-------------------------------------+
1 row in set (0.00 sec)
```

