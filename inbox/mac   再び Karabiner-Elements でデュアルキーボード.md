#mac

---
2021-10-01

# Karabiner-Elements でデュアルキーボード

以前使っていたが削除した。

マウスカーソルが勝手にクリックされたり、画面がちらちらしたり、
ブラウザ入力が困難になったりの原因になってることが分かったからだ。

しかも削除が出来ない部分もあって、クリーンインストールでやっと消せた。

あまりいい思い出がない。
ないが、今でもメンテされていてサポートがBig Sur でも M1 にも対応しているようだ。


https://zenn.dev/dyoshikawa/articles/5b361410de41fb

JIS キーボードの認識が必要らしい。

・Country Code を 45 にする。

・環境設定 -> キーボードの種類を変更 -> JIS



## またデュアルでいってみたくなった

セパレートキーボードに踏み切れない。また、デュアルの思いをしてみたくなった。

https://karabiner-elements.pqrs.org/

karabinar-element アプリを開く。

セキュリティを解除

![[Pasted image 20211001145358.png]]

karabinergrabber と karabinerobserver を入力監視でチェックを入れる。

![[Pasted image 20211001145503.png]]

JIS キーボードにする。

メニューアイコン--> preference -> Virtual keybosard

Cuntry code  45 にする。

うまくいった。

Misc に Uninstall があるから安心か。

### Wacom  ドライバーエラー

一旦アンインストールして、新しいやつをダウンロードしてインストールした。

アクセシビリティにチェックは入ってる。

入力監視、ここにもチェックが必要だった。

![[Pasted image 20211001154005.png]]

OK!!


