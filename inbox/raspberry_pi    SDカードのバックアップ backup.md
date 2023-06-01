#raspberry_pi 


# システムのバックアップ backup

raspberry pi の SDカード のバックアップは rpi-clone を使ってやることにしよう。



## インストール

[raspberry_pi  SDカードのバックアップに rpi-clone を使う](https://www.notion.so/raspberry_pi-SD-rpi-clone-e33dc46a204340fca34083d1cd226c54)

```shell
$ git clone https://github.com/billw2/rpi-clone.git
$ cd rpi-clone
$ sudo cp rpi-clone rpi-clone-setup /usr/local/sbin
```


## バックアップ先SDカードセット

```shell
$ lsblk
```
![](image-kmjxla0u.png)

sdb1,2がそれっぽいので、
sdb のようだ。


## バックアップコマンド実行

最近マウント先が変わったらしく、このままやると失敗する。
ので、手動で umount してからすると上手くいく。

```shell
$ lsblk
デバイス名確認 sda だったとする

$ sudo umount /dev/sda1
$ sudo umount /dev/sda2
```

```shell
$ sudo rpi-clone sda      # <--バックアップ先

Booted disk : mmcblk0
ok clone ?
:yes

ディスク名つける?

```

## アンマウント

/media にぶら下がってるから <-- /media/pi の下につくようになってるようだ。

```bash
$ sudo umount /media/pi/USB名
```

マウントされていなように見えてても、抜くと注意される。そんなときはデバイス名で。

```bash
$ sudo umount /dev/sdc1
```

[[raspberry_pi  システムSDカードのバックアップ]]