---
type: note
---

#pop_os #raspberry_pi 

---
2023-03-06  16:07

# pop_os Raspberry_pi に VNC 接続したい

raspberry_pi の Avahi で VNC の解決がしたい。

VNC over Avahi

VNC connect は解決できなかった。
TigerVNC が解決できたので、これで接続する。

```shell
$ sudo apt install tigervnc-viewer
```

alt キーで tiger とか、remote で TigerVNC が起動出来る。

![[Pasted image 20230306161833.png]]

接続できた。

