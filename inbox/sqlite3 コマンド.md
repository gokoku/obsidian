---
type: note
---

#rails #sqlite 

---
2022-11-09  11:07

# sqlite3 コマンド

インタプリタ起動。
```shell
$ sqlite3 db/hoge.sqlite3
sqlite>
sqlite> .exit
```

rails から入る。

```shell
$ rails dbcondole

sqlite> select * from users limit 10;

テーブル一覧
sqlite> .tables

テーブルスキーマの確認
sqlite> .schema users;

テーブルデータクリア
sqlite> delete from users;


データベースクリアはいらない、ファイルを削除すればいいとのこと
sqlite> .database


sqlite> .exit
```




