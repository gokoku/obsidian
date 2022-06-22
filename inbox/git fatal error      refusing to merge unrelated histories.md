#command #command #github

---
2021-12-27

# fatal: refusing to merge unrelated histories

git でpush しようとしてもエラーになった。

```shell
$ git checkout gh-pages

$ git merge master

$ git push origin gh-pages
fatal: refusing to merge unrelated histories
```


https://qiita.com/mei28/items/85bc881ac1f26332ac15


```shell
$ git merge --allow-unrelated-histories origin/gh-pages



```