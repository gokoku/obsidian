---
type: note
---

#lisp 

---
2022-06-08  13:07

# Emacs で Common Lisp REPL を走らせたい

mondo とやらを入れてみる。

```shell
$ sudo apt install libreadline-dev # pop-os
$ brew install readline   # mac

$ ros install fukamachi/mondo

$ mondo
SBCL 2.2.5 running at 127.0.0.1:50954 (pid=11563)
CL-USER>
```

この先がよくわからん。

## slime を入れる

M-x list-packages --> ac-slime

roswell で slime を installして読み込ませるらしい。
```shell
$ ros install slime
```
.emacs.d/init.el に記述する。

```lisp
(load (expand-file-name "~/.roswell/helper.el"))
```

M-x slime で repl が来た!
見慣れたはずのやつだ!
やっと会えた。

![[Pasted image 20220608161056.png]]
Quicklisp とは pip とか npm にあたるものらしい。

Quicklisp の弱点を解決するために作られたのが qlot とのこと。



