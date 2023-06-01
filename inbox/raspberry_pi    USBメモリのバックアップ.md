#raspberry_pi 

# Mac  でアプリを使おう。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.balena.io/etcher/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.balena.io/etcher/avatar.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">balenaEtcher - Flash OS images to SD cards & USB drives</h1>
		<p class="rich-link-card-description">
		A cross-platform tool to flash OS images onto SD cards and USB drives safely and easily. Free and open source for makers around the world.
		</p>
		<p class="rich-link-href">
		https://www.balena.io/etcher/
		</p>
	</div>
</a></div>




Mac に USB(ext4) を差す。
読めないよどーする?ときかれるが、無視する。

balenaEtcher を使う。

![[Pasted image 20211007113312.png]]

Clone drive をクリック。

![[Pasted image 20211007120232.png]]

どのUSBのイメージがほしいか検討を付ける。

```shell
$ diskutil list

/dev/disk5 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *30.8 GB    disk5
   1:                      Linux                         24.7 GB    disk5s1
   2:                      Linux                         2.1 GB     disk5s2
   3:                      Linux                         1.1 GB     disk5s3
   4:                      Linux                         2.8 GB     disk5s4

/dev/disk6 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *30.8 GB    disk6
   1:                      Linux                         30.8 GB    disk6s1
```

これが ext4 USB で、disk5 が元、disk6 に丸コピしたい。

### コピー元設定

![[Pasted image 20211007120525.png]]

### コピー先設定

![[Pasted image 20211007120605.png]]

Flash 実行。

![[Pasted image 20211007120716.png]]

# Mac コマンド編

USBメモリをSDカードにバックアップしたい。

1つは3つにパーティションを区切っているので、ディスクまるごとコピーがいい。

ディスクイメージを作ればいいかな。

dd コマンドをつかうことにした。
```shell
USBがどのデバイス名か調べる

$ diskutil list

/dev/disk5 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *30.8 GB    disk5
   1:                      Linux                         24.7 GB    disk5s1
   2:                      Linux                         2.1 GB     disk5s2
   3:                      Linux                         1.1 GB     disk5s3
   4:                      Linux                         2.8 GB     disk5s4
```

/dev/disk5 だ。

サイズを調べる。
```shell
$ sudo fdisk /dev/disk3

 #: id  cyl  hd sec -  cyl  hd sec [     start -       size]
------------------------------------------------------------------------
 1: EE 1023 254  63 - 1023 254  63 [         1 - 1953525163] <Unknown ID>
 2: 00    0   0   0 -    0   0   0 [         0 -          0] unused
 3: 00    0   0   0 -    0   0   0 [         0 -          0] unused
 4: 00    0   0   0 -    0   0   0 [         0 -          0] unused
 ```
 
 start + size らしい。
 1 + 1953525163 = 1953525164
 
 で指定する。
 
```shell
$ sudo dd if=/dev/rdisk3 of=/Users/george/Desktop/tono_usb_1.img count=1953525164 bs=1m

```

if が input
of が output

ctrl + T で途中経過が見られる。

時間かかりすぎ!!!


