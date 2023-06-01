#mysql #commnad


---
2022-02-04  18:53

# mysql  全 DB を Drop する

例えば、ogal_ * みたいな形でDBを全削除したい。

```shell
$ mysql -u root -ppassword -e 'show databases' | grep ogal_ | xargs -I "@@" mysql -uroot -ppassword -e "drop database \`@@\`"
```

これでうまくいった。

sql ファイルをまとめて import したい。
フォルダーの中にある sql ファイル。

