#chrome_book #android

Android Studio をダウンロードする。

ダウンロード先が ~/Downloads になってたから、失敗してた。
ChromeOS でマウントしてるからかな。

~/Desktop にしたらダウンロードされるようになった。

```shell
$ tar -zxvf androd-studio-ide.....tar.gz

$ sudo mv android-studio /usr/local/
$ cd /usr/local/android-studio/bin

$ ./studio.sh
```

エミュレータが動かない。

/dev/kvm device: permissiondenied.


https://senooken.jp/post/2020/03/23/

/dev/kvm の所有者を変えると動くらしい。
```shell
$ sudo chown $USER /dev/kvm
確かに動いた。
でもきえる
```

KVM をインストールするそうだ。
```shell
$ sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients brigde-utils
```


kvm グルーブに自分を加える。
```shell
確認
$ grep kvm /etc/group

$ sudo adduser $USER kvm
```
これで恒久的に解決とのことだが、治らず。

libvirt は仮想化管理用共通APEを提供する red-hat 系OSS
仮想I/Oデバイスドライバ

![](20201220002650-knz9t5ms.png)