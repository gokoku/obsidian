#docker #mysql #db 

---
2022-01-26  22:03

# docker  MySQL5.7 を立てる

T氏の作った優秀なパッケージがあるので、コピーして勉強する。

## Dockerファイル構成

```text
.
├── docker
│   └── mysql
│       ├── Dockerfile
│       └── conf.d
│           └── my.cnf
└── docker-compose.yml
```

## 設定ファイル

### docker-compose.yml

```yaml:docker-compose.yml
version: '3.3'
services:
	db:
		build: ./docker/mysql
		restart: always
		environment:
			MYSQL_ROOT_PASSWORD: root
		ports:
			- "3306:3306"
		volumes:
			- ./docker/mysql/initdb.d:/docker-entrypoint-initdb.d
			- ./docker/mysql/config.d:/etc/mysql/conf.d
			- ./var/log/mysql:/var/log/mysql
			- ./var/lib/mysql:/var/lib/mysql
```

volumes でホストのHDに内容が残るように永続化している。

### mysql/Dockerfile

```dockerfile:mysql/Dockerfile
FROM mysql:5.7
RUN touch /var/log/mysql/mysql.log # 指定場所にログを記録するファイルを作る
```

### mysql/conf.d/my.cnf

```config:mysql/conf.d/my.cnf
[mysqld]
#character-set-server=utf8mb4
# mysqlサーバー側が使用する文字コード

explicit-defaults-for-timestamp=1　  
# テーブルにTimeStamp型のカラムをもつ場合、推奨

general-log=1　                  
# 実行したクエリの全ての履歴が記録される（defaultではOFFになっているらしい）

general-log-file=/var/log/mysql/mysqld.log 
# ログの出力先

[client]
#default-character-set=utf8mb4
# mysqlのクライアント側が使用する文字コード
```


## 起動

```shell
$ cd somepath
$ docker-compose up&
```