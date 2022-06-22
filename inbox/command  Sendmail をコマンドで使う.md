---
type: note
---

#command

---
2022-05-06  13:42

# command  mail をコマンドで送信する

### sendmail で送る
```shell
$ sendmail -t george.6809@gmail.com
Subject: "test"
This is a test of echo.
```
ctrl-D で送信。

![[Pasted image 20220506135739.png]]



### postfix で送る

```shell
$ sudo apt install postfix

$ echo "test message" | mailx -s 'test subject' george.6809@gmail.com
```

来た。

![[Pasted image 20220506134927.png]]

