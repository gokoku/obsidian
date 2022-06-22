#command #git

---
2021-06-20

# Tag も一緒に Push する

https://qiita.com/aki_55p/items/530754ac6e861122f29b

```shell
全部
$ git push origin --tags

個別
$ git push origin tag-name

リモートから削除
$ git tag -d tag-name
$ git push origin :tag-name

もしくは
$ git push --delete origin tag-name
```

