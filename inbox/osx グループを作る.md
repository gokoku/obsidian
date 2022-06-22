---
type: note
---

#osx #command

---
2022-06-15  08:45

# osx グループを作る

一覧
```shell
$ dscl . ls /Groups
```

グループ作成
```shell
$ sudo dscl . -create /Groups/ftpgroup PrimaryGroupID 1000

# user グループに追加
$ sudo dscl . -append /Groups/ftpgroup GroupMembership people2

# 確認
$ dscl . -read /Groups/ftpgroup


# 削除
$ sudo pure-pw userdel people2
```

