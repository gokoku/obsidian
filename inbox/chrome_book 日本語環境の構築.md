#chrome_book 

[https://okayu-moka.hatenablog.com/entry/2020/03/14/180000](https://okayu-moka.hatenablog.com/entry/2020/03/14/180000)

```jsx
$ sudo apt update
$ sudo apt upgrade -y
$ sudo apt install -y task-japanese locales-all fonts-noto-cjk
$ sudo localectl set-locale LANG=ja_JP.UTF-8 LANGUAGE="ja_JP:ja"
$ source /etc/default/locale

$ sudo apt install -y fcitx-mozc fcitx-frontend-all
$ fcitx-autostart
$ fcitx-configtool
```

/etc/systemd/user/cros-garcon.service.d/cros-garcon-override.conf に追記

```jsx
Environment="GTK_IM_MODULE=fcitx"
Environment="QT_IM_MODULE=fcitx"
Environment="XMODIFIRES=fcitx"
Environment="XMODIFIERS=@im=fcitx"
```

~/.somelierrc に追記して自動起動

```jsx
/usr/bin/fcitx-autostart
```

日本語入力出来る mate-terminal がいいらしい。

```jsx
$ sudo apt install mate-terminal
```

---

Cica フォントがいいらしい。

```jsx
$ mkdir ~/cica
$ cd ~/cica
$ wget https://github.com/miiton/Cica/releases/download/v5.0.1/Cica_v5.0.1_with_emoji.zip
$ unzip Cica_v5.0.1_with_emoji.zip
$ rm Cica_v5.0.1_with_emoji.zip
$ sudo mv Cica*  /usr/share/fonts/
$ fc-cache -f -v
$ cd ~/
$ rm -rf ~/cica

```

IPAフォントでも入れる。

```jsx
$ sudo apt install fonts-ipafont fonts-ipaexfont
```

言語と入力方法で、英数字(日本語)キーポードで有効にするとターミナルでも日本語が出来るようになってた。

でも、Visual Studio Code では Mozc でないと日本語が入力されないので、fcitx は必要。
emacs でも fcitx が必要だ。
デフォルトで Ctrl - space でトグル。

でも、設定がアプリアイコンから fcitx をつつくといつまでもクルクルインジケータが回り続ける。

[https://cloud-work.net/linux/fcitx-mozc/](https://cloud-work.net/linux/fcitx-mozc/)

どうやって設定するのだろうか。

```jsx
$ fcitx-configtool
```

入力メソッドのオンオフのキー設定が出来る。

ここでの入力メソッドのホットキーは空にした(returnのみで空になる)

## Mozc の設定

```jsx
$ /usr/lib/mozc/mozc_tool --mode=config_dialog
```

ここでかな入力を設定する。

ローマ字入力
ATOK
にした。

かなキーで日本語入力、英数キーでアルファベッドになって、かなりいい感じになった!!!


---
# Ubuntu

```shell
$ sudo apt install fonts-takao

$ export LANG=ja_JP.UTF-8
$ sudo locale-gen $LANG
$ sudo update-locale $LANG
```

setting --> appereance --> Fonts --> TakaoPゴシック

```shell
$ sudo apt install ibus ibus-kkc
```

/etc/profile
```shell
export GDM_LANG=ja
export GTK_IM_MODULE=ibus
export XMODIFIERS=@im=ibus
export QT_IM_MODULE=ibus
```

setting --> sesseion and Startup で ibus-daemon を追加した。
command : /bin/ibus-daemon

なぜかこれでないと起動時にメニューにあらわれてくれない。



キーボード追加してJISにしたい。
setting で keyboard で Japanese に取り替えた。

MOZC が不安定で動かなかったので、KKC をつかうことにした。
タイピング方式に Kana があった!!!!





ctrl と search を取り替えたい。
```shell
$ /usr/bin/setxkbmap -option "ctrl:swap_lwin_lctl"
```

ログアウトするとなくなるので、/etc/default/keyboard に記述したが、機能せず。
.bsashrc に記述した。

