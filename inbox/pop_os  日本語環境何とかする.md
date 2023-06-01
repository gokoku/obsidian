---
type: note
---

#pop_os 

---
2022-05-07  21:37

# pop_os  日本語環境何とかする

```shell
$ sudo apt update
$ sudo apt upgrade -y
$ sudo apt install -y task-japanese locales-all fonts-noto-cjk
$ sudo localectl set-locale LANG=ja_JP.UTF-8 LANGUAGE="ja_JP:ja"
$ source /etc/default/locale

$ sudo apt install -y fcitx-mozc fcitx-frontend-all
$ fcitx-autostart

ここでエラー
XIM  開始エラー: fcitx という他の XIM が動いていませんか?

$ fcitx-configtool
```

fcitx-mozc fcitx-frontend-all を remove して、iBUS に戻す。
日本語入力出来ればそれでいいということで。

```shell
$ sudo apt remove fcitx-mozc fcitx-frontend-all
```

iMac で日本語の入力が出来ない。
Zenkaku_Hankaku が無いから

[[pop_os キーボードのレイアウトを変更する 2]]

これでOK!!