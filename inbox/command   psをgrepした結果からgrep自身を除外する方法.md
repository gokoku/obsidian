#command 

---
2022-02-02  16:22

# command   psをgrepした結果からgrep自身を除外する方法

https://webkaru.net/linux/ps-grep-exclude/

更に grep して grep がついてるのを排除。

```shell
$ ps aux | grep httpd | grep -v grep
```


\[\] を利用。

```shell
$ ps auxf | grep [h]ttp
```