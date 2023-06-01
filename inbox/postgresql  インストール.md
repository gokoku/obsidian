#postgresql

---
2021-07-15

# Postgresql install

postgresql の代わりに postgresql@14使えと言われた。2023/04/01


```shell
$ brew uninstall postgresql
$ brew install postgresql@14
```


```shell
$ brew install postgresql

サーバースタート
$ brew services start postgresql

ログイン
$ psql postgres

postgres=#
```

# role "postgres" does not exist と出る不具合

https://qiita.com/Ksk8358/items/6629f597ddf7f4ccd97c

```shell
$ psql postgres

> \l
                          List of databases
   Name    | Owner  | Encoding | Collate | Ctype | Access privileges
-----------+--------+----------+---------+-------+-------------------
 postgres  | george | UTF8     | C       | C     |
 template0 | george | UTF8     | C       | C     | =c/george        +
           |        |          |         |       | george=CTc/george
 template1 | george | UTF8     | C       | C     | =c/george        +
           |        |          |         |       | george=CTc/george


role の状態を確認する

> \du
 Role name |                         Attributes                         | Member of
-----------+------------------------------------------------------------+-----------
 george    | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
```

ユーザーpostgres がないのが問題なのだそうです。

brew で george ユーザーで入れたから、postges が出来なかったようだ。


shell から role "postgres" を作成するコマンドを打つらしい。

```shell
$ createuser postgres
```

確認する

```shell
$ psql postgres

> \du

                                   List of roles
 Role name |                         Attributes                         | Member of
-----------+------------------------------------------------------------+-----------
 george    | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 postgres  |

```

これでよし!!

でもなかった。

# permission denied to create database

brew install postgresql  で作ったら、Superuser が george になってたのが問題だった。

createuser で --superuser 権限を与えて作るのがいいようだ。

```shell
一旦削除する

$ dropuser postgres

--superuser 権限つけて作る

$ createuser postgres -s
```

## データベース delete
```shell
> DROP DATABASE name;
> \l

コンマつけないと動かないで、スルーされるので注意。
```



