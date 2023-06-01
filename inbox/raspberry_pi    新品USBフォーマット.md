#raspberry_pi 


ext4 でフォーマットする。

その前にアンマウント。

```shell
$ sudo lsblk
sda           8:0    1 28.7G  0 disk
└─sda1        8:1    1 28.7G  0 part /media/pi/265C-386C

$ sudo umount /media/pi/265C-386C
```

```shell
$ sudo fdisk -l
デバイスを特定する

例えば
Disk /dev/sda: 28.7 GiB, 30765219840 bytes, 60088320 sectors
Disk model:  SanDisk 3.2Gen1
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x00000000

Device     Boot Start      End  Sectors  Size Id Type
/dev/sda1          32 60088319 60088288 28.7G  c W95 FAT32 (LBA)

FAT32 になっている。
この場合は、/dev/sda


アンマウントする
$ sudo umount ....
```

特定したデバイスに fdisk で入る。
```shell
$ sudo fdisk /dev/sda
> p で様子見

Disk /dev/sda: 28.7 GiB, 30765219840 bytes, 60088320 sectors
Disk model:  SanDisk 3.2Gen1
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x00000000


> d    #でパーティション削除
> d 
> w     #書き込み

ここでは、パーティションはなかったようだ
```

新たにパーティションを作る

```shell
$ sudo fdisk /dev/sdc
> n
> p   基本パーティション
> 1   パーティション1個
> リターン  始まり
> リターン　終わり
> p     確認して、
> w      書き込み
```

/def/sdc1 がバーティションで作られた。
あれ? ここまでいらなかったかな。sda1 が元々あったし。

ext4 でフォーマットする。
```shell
$ sudo mkfs.ext4 /dev/sdc1
 ```
 
マウントする。
```shell
$ mkdir /tmp/backup_usb
$ sudo mount /dev/sdc1 /tmp/backup_usb
$ sudo chown -R pi:pi /tmp/backup_usb
```



 


