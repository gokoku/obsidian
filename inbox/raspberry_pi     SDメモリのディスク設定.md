#raspberry_pi 

デバイス情報を見たい。

```bash
$ sudo fdisk -l
```

Mac は diskutil list

マウント状況を見たい。

```bash
$ sudo df -h
```

SDメモリのUUID

```bash
$ sudo blkid /dev/sda1
```

### /etc/fstab の設定(2022/3/10 現在)
- /log --> /var/log
- /home/pi  --> /home
/home/pi にマウントでは Bullseye でマウント失敗する。
/home にマウントするようにしたらうまく動くようになった!

```shell
$ cd /media/pi/032460a46-.....
$ cp -R /home/* .
```


```bash
proc            /proc           proc    defaults          0       0
PARTUUID=2463978e-01  /boot           vfat    defaults          0       2
PARTUUID=2463978e-02  /               ext4    defaults,noatime  0       1

UUID=32460a46-2430-4c24-9c78-47b26e0f746c /home　　 ext4 defaults,noatime 0 0
UUID=0330c82a-9519-4456-ae6a-81e55cba6cfb /tmp     ext4 defaults,noatime 0 0
UUID=3c716c57-4cd7-43fc-9bc4-00afdb65221d /var/tmp ext4 defaults,noatime 0 0
UUID=f9f3f9b7-2426-4671-aec0-2a76e6938d3b /var/log ext4 defaults,noatime 0 0
```

/log から /var/log にする。



# バックアップの話

この 外部メモリ SDメモリ、USBメモリディスク丸ごとコピーで、UUID も一緒になるようだ。

