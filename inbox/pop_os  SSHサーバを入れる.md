---
type: note
---

#pop_os #chrome_book 

---
2022-05-09  23:27

# pop_os  SSHサーバを入れる

```shell
$ sudo apt install openssh-server

$ sudo emacs /etc/ssh/sshd_config
```

コメントインするところ。

```
Port 22
```

```shell
$ sudo systemctl restart sshd

$ ssh george@pop-os.local

> hostname
pop-os
```

