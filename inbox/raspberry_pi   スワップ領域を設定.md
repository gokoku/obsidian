#raspberry_pi 

apt がスワップで失敗する。

ちょっとだけ割り当てたい。

/etc/dphys-swapfile

```bash
CONF_SWAPSIZE=2048

CONF_MAXSWAP=2048
```

終わったら 0 にする。

firefox の延命サポート版

```bash
$ sudo apt-get install iceweasel
```

さっきまで失敗してたが、動いた。

