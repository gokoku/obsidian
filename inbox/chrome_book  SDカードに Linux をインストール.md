#chrome_book 

https://chromesoku.com/linux-on-chromebook-sdcard/

試しにやってみよう。

デベロッパーモードになる。

デバッグモードになる。
このときのパスワードが root のパスワードになる。
* ctrl + Alt + F2(->) でターミナルになる。
* root で login する。password をさっきのもので入る。
* chromeos-setdevpasswd で sudo のパスワードをセットする。

これで、sudo でコマンドが使える様になる。

### crouton をダウンロードする

```shell
https://goo.gl/fd3zc
```

### SDカードに Linux をインストールする

```shell
crosh > shell
chronos@localhost / $ lsblk


$ cd /media/removable\ SD\ Card

$ ~/Downloads/crouton -r trusty -t xfce -p /media/removable\SD \Card

```

### ボリュームラベル

```shell
$ e2label /dev/sda hoge

確認
$ e2label /dev/sda

消去
$ e2label /dev/sda ""
```