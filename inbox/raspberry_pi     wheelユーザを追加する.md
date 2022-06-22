#raspberry_pi 


raspberry pi には wheel がない。

wheel ユーザを追加する。

```bash
$ sudo adduser wheel
```

既定値のままなら全部 return

sudo グループに追加する。

```bash
$ sudo grasswd -a wheel sudo
ユーザ wheel をグループ sudo に追加
```

