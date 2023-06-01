#raspberry_pi 


[https://qiita.com/karaage0703/items/ed18f318a1775b28eab4](https://qiita.com/karaage0703/items/ed18f318a1775b28eab4)

とりあえず、手軽に /etc/rc.local にしようか。

```bash
/etc/rc.local

ここにコマンドスクリプトを書く。
```

/var/log/boot.log に失敗したかどうか見られる。

どうもPythonスクリプトは動いてない。

---

cron でも出来るらしい。

```bash
$ crontab -e

@reboot    /home/pi/bin/hoge.sh
```

どうもスクリプトは動いてない。

---

autostart を使ってみる。

GUI 関係はこれでうまくいくようだ。

```bash
$ mkdir -p ~/.config/lxsession/LXDE-pi

$ cp /etc/xdg/lxsession/LXDE-pi/autostart ~/.config/lxsession/LXDE-pi/
```

~/.config/lxsession/LXDE-pi/autostart

```bash
@lxpanel --profile LXDE-pi
@pcmanfm --desktop --profile LXDE-pi
@xscreensaver -no-splash

/home/pi/bin/images_classification.sh
```

うまくいった。

このプロセスを終了させたい。

プロセスID を調べて kill するしかないみたい。

---
時々起動しそこねるときがある。
たぶん、反応して、localhost に投げようとして node-red が立ち上がってないときの気がする。

node-red を止めて、pythonスクリプトを動かしてみたが、普通に動いた。
よって違うようだ。
謎。

