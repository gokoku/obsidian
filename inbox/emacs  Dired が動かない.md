#emacs

---
2021-08-01

# Listing directory failed but 'access-file' worked

DIred が動かない。

プライバシーで引っかかってるらしい。

![[Pasted image 20210801183040.png]]

/usr/bin/ruby をフルアクセスで許可する。

因みに ruby は

```shell
$ which ruby
/Users/george/.asdf/shims/ruby
```

なのだが、emacs では /usr/bin/ruby を使うらしい。

