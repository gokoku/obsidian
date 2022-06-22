#raspberry_pi 

---
2022-03-08  16:04

# raspberry_pi   real-VNC のアップデートポップアップが出てどーにかしたい

![[Pasted image 20220308160514.png]]

メニューも出てないし。
```shell
$ apt list | grep readvnc

realvnc-vnc-server/oldstable,now 6.7.2.42622 armhf [インストール済み]
```

一旦 remove しても これが入る。oldstable ってなんだ?

Bullseye が stable になったから Buster は oldstable ということらしい。

結局 Bullseye にすることにした。

