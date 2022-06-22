#chrome_book  #ubuntu


Debian buster を入れたが、Docker が動かず、詰んだ。
anyenv はバッチリだったのに。

crouton の構造上の壁だったのだろうか、ということで、Ubuntu を入れてみて様子を見ることにした。
ここで、目標が anyenv と Docker と VSCode と Emacs と Zsh ということになった。

https://qiita.com/htnk/items/6458ec6412210406486b
xiwi が動かなかった。


https://github.com/dnschneid/crouton/wiki/3-Steps-to-Install-Bionic-Beaver


## USB まっさらにする

```shell
$ sudo umount /media/removable/USB\ Drive
$ sudo mkfs -t ext4 /dev/sda
$ sudo mount /dev/sda /media/removable/USB\ Drive/
```
## crouton で Ubuntu 20.04 をインストール


```shell
$ sudo mount -o remount,symfollow,exec /media/removable/USB\ Drive

$ sudo crouton -r focal -t core,audio,cli-extra -p /media/removable/USB\ Drive
```

Ubuntu に入る。
```shell
$ sudo /media/removable/USB\ Drive/bin/enter-chroot

> sudo apt install xterm xinit
```
ctrl + D で抜ける。

```shell
$ sudo crouton -r focal -t chrome,extension,keyboard,x11,xfce,xorg -u -p /media/removable/USB\ Drive


$ sudo /media/removable/USB\ Drive/bin/startxfce4
動いた!!
```

apt がエラー。
chrome絡みのエラー。
https://sicklylife.hatenablog.com/entry/2017/08/08/193118
```shell
$ wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add - 
$ sudo apt update  
```



## chrome download はこうする

Chrome OS でダウンロードする。共有にする(しなくても??) 
Crouton Xubuntu の $HOME/Downloads で共有される!!!



### クリックじゃなくてタッチにしたい

```shell
$ sudo apt install xserver-xorg-input-synaptics
```
Tap to Click うまくいった!!
けど、感度が良すぎて使いづらい、しかもスクロールが順方向になってしまった。

```shell
$ sudo apt remove xserver-xorg-input-synaptics
```
クリックはしないといけないが、スクロールは逆方向に戻った、てゆーか、setting 通りになった。






これなんだろ。気になる。
```shell
sudo crossystem dev_boot_signed_only=1   こっちが non-developer mode ?
とか
sudo crossystem dev_boot_usb=1 dev_boot_signed_only=0
```

