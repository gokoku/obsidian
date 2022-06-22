#command #git

---
2021-08-25

# fatal: refusing to merge unrelated historiesを解決する話

gitlab に上げようとした。が、github とかからの import か new repository かだったので、new を作った。

```shell
$ git pull origin main

hint:   git config pull.rebase false  # merge (the default strategy)
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only

fatal: refusing to merge unrelated histories
```

merge の動きをしてほしいので、

```shell
$ git config pull.rebase false

$ git pull origin main

From gitlab.alfa-people.ml:george/people-shop-top-animation
 * branch            main       -> FETCH_HEAD
fatal: refusing to merge unrelated histories
```


https://qiita.com/mei28/items/85bc881ac1f26332ac15

お互いの main ブランチが先祖が違うからマージしないとのことらしい。

なので、そこを許してくれとオプションをつける。

```shell
$ git merge --allow-unrelated-histories origin/main

CONFLICT

解消

$ git add .
$ git commit
$ git push origin main
```

OK!!!

gitlab では README.md を作らないでローカルで作っちゃおう。

