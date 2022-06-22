#chrome_book 

---
2021-05-31

# Chrome book 側が host

しかし、chrome book では 読み取り専用となってformat出来ないようだ。

ハード領域はなんとも手が出せないようだ。

# Crouton Ubuntu で USBツール

lsusb を使えるようにする。

```shell
$ sudo apt install usbutils
```

ディスクユーティリティをインストールする。

```shell
$ sudo apt install gnome-disk-utility

#起動
$ gnome-disks
```

ディスクユーティリティが起動する。
が、ハード系はゴニョゴニョ出来ないようだ。

