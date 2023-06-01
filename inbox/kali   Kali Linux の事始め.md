#kali

---
2021-11-15

# Kali Linux の事始め

ここが、今の情報。

https://qiita.com/sireline/items/733390ad025febd407f0

```shell
$ sudo apt update
$ sudo apt upgrade
```

Download

https://www.kali.org/get-kali/[[1572305786534-030ce714-cc3b]]

ディスプレイはサブモニターで丁度良くなる。Mac 本体だとものすごく小さくなる。設定できないくらい小さくなる。

## パスワード変更

```shell
$ sudo passwd kali
```

cpu68060


## 日本語化はしないことにした

```shell

$ locale


$ sudo dpkg-reconfigure locales
```

![[Pasted image 20211115210142.png]]

ja_JP.UTF-8 UTF-8 を選択。space で2回ほど再起動すると適応されるらしい。

![[Pasted image 20211115211050.png]]

古い名前のままにするをクリック。

でもやめよう。英語環境で。en_US.UTF-8 UTF-8

## タイムゾーンを設定

![[Pasted image 20211115214013.png]]

右上メニュー-->時計-->右クリック-->Properties

Asia/Tokyo



## 日本語入力

```shell
$ sudo apt install fcitx-mozc
$ suod reboot
```

![[Pasted image 20211115214703.png]]

Setting -> Fcitx Configuration で設定した。

https://qiita.com/sireline/items/733390ad025febd407f0

+で keyboard を追加で、 Only Show CurrentLanguage チェック外すとMozc  が出てくる。

![[Pasted image 20211115215913.png]]

![[Pasted image 20211115215937.png]]

キーボードマークで Japanese を選択した。

Setting -> Mozc Setting -> inputmode を Kana


日本語入力出来るようになった。変換キーがトグルで日本語英数切り替え出来るからいいか。

会社のmac ではカナキーで上手くトグルってくれなかったので、windows keyboard の メニューキーにした。



## キーボードの日本語配列

Setting -> Keyboard --> Layout で Use system defaults のチェックを外す。

Keyboardlayout に追加で Japanese(OADG109A)がいいみたいだ。



