---
type: note
---

#emacs

---
2022-05-21  08:58

# emacs Brew で入れたら CUI

家のmac クリーンインストールした。
--cask 付けないと普通に CUI だ。

```shell
$ brew install emacs

# CUI terminalだ
$ emacs
```

```shell
$ brew services restart emacs

$ ps aux | grep emacs
george           13923   0.0  0.1 34155088  13732   ??  S     9:02AM   0:00.03 /usr/local/opt/emacs/bin/emacs --fg-daemon
```

daemon  で動いてるのか、emacsclient で起動できる。

```shell
$ emacsclient .
```

てゆーか、もはや emacsclient 状態？




### service
```shell
$ brew services start emacs
$ brew services stop emacs
$ brew services restart emacs
```


