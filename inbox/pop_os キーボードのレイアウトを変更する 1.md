---
type: note
---

#pop_os 

---
2022-05-07  21:57

# pop_os キーボードのレイアウトを変更する 1

```shell
$ sudo emacs /etc/default/keyboard
```

keycode をこれで確認出来る。

```shell
$ xev
```

~/.Xmodmap を作って書き込む。

chromebook の super がイベントで取られて、keycode が分からん。

Gnome-tweaks( gnome-tweak-toll) でなんとか出来た。
```shell
$ sudo apt install gnome-tweaks

$ gnome-tweaks
```

キーボードとマウス --> キーボード --> アクティビティ画面のショートカット

このなんのことかわからない項目の 左Super を 右Super にしたらイベントに取られなくなった。

```shell
$ xev

keycode 133 (keysym 0xffeb, Super_L)  # super の位置のキー

keycode 64 (keysym 0xffe9, Alt_L)     # alt の位置のキー

keycode 37 (keysym 0xffe3, Control_L) # ctrl の位置のキー
```

Super のコードがわかった。

### 整理してみる

- Super_L --> Alt_L          の位置
- Alt_L   --> Control_L     の位置
- Control_L --> Super_L の位置

これを .Xmodmap で何とかするのがいいかな。

xmodmap には修飾キーのグループがあって、それを変更する必要があるらしい。

https://blue-red.ddo.jp/~ao/wiki/wiki.cgi?page=xmodmap%A4%CE%BD%F1%A4%AD%CA%FD

修飾キーグループを見る。

```shell
$ xmodmap

control     Control_L
mod1        Alt_L
mod4        Super_L


$ xmodmap -pke
keycode...
```

3つがお互いに回転するように入れ替わるようにするには

~/.Xmodmap
```
remove Control = Control_L
remove Mod1 = Alt_L
remove Mod4 = Super_L

keysym Super_L = Control_L
keysym Control_L = Alt_L
keysym Alt_L = Super_L

add Control = Control_L
add Mod1 = Alt_L
add Mod4 = Super_L
```

```
$ xmodmap ~/.Xmodmap
```

OK のようだ。

### 起動時に設定されるようにする

自動起動するアプリケーションの設定を起動する。

```shell
$ gnome-session-properties
```

追加で
- 名前 : ExchangeKeys
- Command : /usr/bin/xmodmap /home/george/.Xmodmap
- 説明 : Ctrl key -> Super key -> Alt key -> Ctrl key に key を割り当てる

追加する。

これで ~/.config/autostart に xmodmap.desktop というファイルが生成されて、以上の設定がされるようになってるのが確認できた。
が、これでは上手くいかなかった。

autostart 自体は touch test_test とサンプル作ってやってみたが、動いている。
しかし、xmodmap が動いてない。

「/usr/bin/xmodmap /home/george/.Xmodmap」をターミナルでコピペすると、動く。

#### shell script を作ってそれをアプリとして起動させる

<div class="rich-link-card-container"><a class="rich-link-card" href="https://raspida.com/pop_os-keybind-mac" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://raspida.com/wp-content/uploads/popos-keybind-title.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Pop!_OSでMacかな・英数キー有効、CommandキーをCtrlキーに変更 | ラズパイダ</h1>
		<p class="rich-link-card-description">
		Pop!_OSを古いMacBookProでデュアルブートにして使っています。個人的にお気に入りのPop!_OSでプチ不満だったのが日本語入力のキーマップです。 WindowsPCならキーボードの全角/半角キーで事足りますが、Macだとそのキ
		</p>
		<p class="rich-link-href">
		https://raspida.com/pop_os-keybind-mac
		</p>
	</div>
</a></div>


この人も、autostart の .desktop ファイルで上手くいかなかったとのこと。

~/xmodmap-remap.sh
```shell
#!/bin/bash
sleep 10
xmodmap $HOME/.Xmodmap
```

```shell
$ chmod +x xmodmap-remap.sh
```


Dock の Show Applications --> System --> 自動起動するアプリケーション

って、gnome-session-properties じゃん。

コマンドのところで、このshell スクリプトを選んであげればいいのか。

- Name : Startup keybind
- Command : /home/george/xmodmap-remap.sh
- Comment : キーバインドを変更する

再起動する。

上手くいった!!

Mac keyboard と一緒な感じになりました(^o^)/

## 上手くいかなくなりました
autostart に google drive mount スクリプトを入れたら、こちらが動かなくなりました。

上手く動くようになった。

sleep 10 がどうも肝心な数字のようです。

けど、不安定なのでこれ止めます。


## これで行きます!
[[pop_os キーボードのレイアウトを変更する 2]]

