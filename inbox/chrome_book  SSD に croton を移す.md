#chrome_book 

## SSD を初期化した。
```shell
$ lsblk

sdb
sdb1     /var/host/media/removable/SSD-PGMU3

$ sudo umount /var/host/media/removable/SSD-PGMU3
$ sudo mkfs -t ext4 /dev/sdb

# 抜き差しした。

$ lsblk
sdb     /var/host/media/removable/USB Drive 1
```

## 全コピーした。
```shell
# もともとubuntuの入ってるUSB
$ cd /var/host/media/removable/USB\ Drive
$ sudo cp -a bin /var/host/media/removable/USB\ Drive\ 1/

$ sudo cp -a chroots /var/host/media/removable/USB\ Drive\ 1/
# すごく時間がかかってる。不安...帰ってきた!!
```

## SSD で起動する

電源落として、元USB を取って SSD にすげ替えて起動してみる。

超うまくいった!!
```shell
$ df -h

/dev/sda            220G
```

わお!! 220G の Linuxマシンになっちまった!!


## 整備する

### ボリューム名を付ける。
umount は chromeOS からやる。
```shell
# e2lable /dev/sda ssd
```
ついでに USBメモリにも名前付ける。
こっちはバッグアップ用にする。

```shell
# e2label /dev/sdb  backup
```

### 設定

/root/.bashrc にalias を設定する。
```shell
alias startxfce4='/media/removable/ssd/bin/startxfce4'
alias remount='/usr/local/bin/remount.sh'
```

因みにユーザの設定場所は、/home/chronos/user/.bashrc
