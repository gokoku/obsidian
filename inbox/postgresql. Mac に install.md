---
type: note
---

#postgresql #osx 

---
2022-12-07  09:12

# postgresql. Mac に install

```shell
$ brew insall postgresql

$ brew services start postgresql

$ psql postgres

postgres をスーパーユーザーにする
> create user postgres SUPERUSER;

これでデータベースも作り放題になる
> create database hogehoge owner=postgres;
> \l
>                            List of databases
   Name    |  Owner   | Encoding | Collate | Ctype | Access privileges
-----------+----------+----------+---------+-------+-------------------
 hogehoge  | postgres | UTF8     | C       | C     |
 postgres  | george   | UTF8     | C       | C     |
 template0 | george   | UTF8     | C       | C     | =c/george        +
           |          |          |         |       | george=CTc/george
 template1 | george   | UTF8     | C       | C     | =c/george        +
           |          |          |         |       | george=CTc/george
```

hogehoge データベースに postgres ユーザ で入る。

```shell
$ psql hogehoge -U postgres

テーブルを作る
> create table account(id varchar(8), name varchar(8));
> \d
>           List of relations
 Schema |  Name   | Type  |  Owner
--------+---------+-------+----------
 public | account | table | postgres
(1 row)
```


