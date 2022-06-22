#ruby/rails  #docker 

---
2022-03-18  11:09

# rails  Docker の MySQL に接続したい

```shell
$ rails db:create

Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
```

database.yml に ```host``` の設定を加える。

```yml:database.yml

default: &default
  adapter: mysql2
  encoding: utf8mb4
  username: root
  password: root
  host: 127.0.0.1

development:
  <<: *default
  database: chagpon_design_workflow_development
```

```shell
$ rails db:create
$ rails db:migrate
$ rails db:seed
```


コマンドで接続するには
```shell
$ myslq -h 127.0.0.1 -P 3306 -uroot -p

$ mysql -h 127.0.0.1 -uroot -p  #<--これでもいい

mysql>
```



