---
type: note
---

#command #pop_os 

---
2022-10-12  10:12

# command グループID とか見たい

/etc/group ファイル

```shell
$ sudo cat /etc/group
```

george グループに hoge を追加する。

```shell
$ sudo gpasswd -d docker george
```