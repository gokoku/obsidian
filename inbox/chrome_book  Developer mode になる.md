#chrome_book 

https://www.wikihow.jp/Chromebook%E3%81%A7USB%E3%83%96%E3%83%BC%E3%83%88%E3%82%92%E6%9C%89%E5%8A%B9%E3%81%AB%E3%81%99%E3%82%8B


Esc + F3(ぐるぐるマーク) + 電源で入る。

Ctrl + D 確認メッセージが出るが、リターン。

真っ暗画面

確認メッセージ　Ctrl + D





Ctrl + Alt + F2(->) で Developer Console になった。

'chronos'で login と sudo が出来る。


```shell
Developer Console

To return to the browser, press:
  [ Ctrl ] and [ Alt ] and [ <- ] (F1)

To use this console, developer mode must be enabled.
Doing so will destroy any saved data on the system.

In developer mode. It is possible to
- login and sudo as user 'chronos'
- require a password for sudo and login(*)
- disable power management behavior (screen dimming):
  sudo initctl stop powerd
- install your own operating system image!

* to set a password for 'chronos', run the following as root:

chromeos-setdevpasswd

If you are having trouble booting a self-signed kernel, you may need to
anable USB booting. To do so. run the following as root:

enable_dev_usb_boot

Have fun and send patches!
```

localhost login:  ときたから、root リターン
localhost ~ # chromeos-setdevpasswd   でパスワードセットした。
cpu68060

これで、chronos ユーザーでパスワードがセットされた。
もう root では入れなくなった。

これで、USB から起動出来るというのか????!!!!!


Xubuntu の起動USB をさして、別口に 64G USB を挿して、行くよ。

```shell
chronos@localhost ~ $     sudo enable_dev_usb_boot
Password: 

    SUCCESS: Booting any self-signed kernel from SSD/USB/SDCard slot is enabled.
    
    Insert bootable media into USB / SDCard slot and press Ctrl-U in developer
    screen to boot your self-signed image.
```

Ctrl-U を押す。変化なし。
```
$ sudo su
# chrossystem --all | grep boot

```
電源長押ししたら元通りのまっさらさら。??

どうもこれが開発者モードらしい。

初期設定画面で「デバッグ機能を有効にする」クリックしたら、再起動して、root のパスワード入力させられた。



Ctrl + Alt + t で crosh をブラウザで起動する。
shell と打って chronos になれば開発者モードとのこと。
```
chrosh> shell
chronos@localhost / $
```




---
## ライブ USB でブートしたい。

普通の chrome book として起動してるので、バーもランチャーもある状態。

Ctrl + Alt + t で chrosh
```shell
$ shell

chronos@localhost / $ sudo crossystem    パラメータ一覧

dev_boot_usb  =  1   になってることを確認
```

もしくは、Ctrl + Alt + F2(->) でコンソールになる。
chronos ユーザでログインする。


コンソールから Ctrl + Alt + F1(<-) でブラウザに戻れる。



ここで ライブUSBを挿した。


SeaBIOS がある機種は Ctrl + L でいけるようだ。
この機種にはないので、外からファームウェアを入れるようだ。


chrouton がいいのか???

crostini は developer だろうと変わりなし。
