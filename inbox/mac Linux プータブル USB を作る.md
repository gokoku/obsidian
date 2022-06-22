#mac

https://www.aizulab.com/blog/mac%E3%81%A7linux%E3%81%AE%E3%83%96%E3%83%BC%E3%82%BF%E3%83%96%E3%83%ABusb%E3%82%92%E4%BD%9C%E3%82%8B/

USBを見つける。
```shell
$ diskutil list

/dev/disk4
```

アンマウントする。
```shell
$ diskutil unMountDisk /dev/disk4
```

ダウンロードしたisoファイルをimg形式に変換する。
```shell
$ hdiutil convert -format UDRW -o ubuntu-21.04-desktop-amd64.img ubuntu-21.04-desktop-amd64.iso
```

imgファイルをUSBBメモリに書き込む。
```shell
$ sudo dd if=/Users/george/Downloads/ubuntu-21.04-desktop-amd64.img.dmg of=/dev/disk4 bs=1m
```

