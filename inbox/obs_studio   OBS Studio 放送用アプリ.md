#obs_studio

---
2021-10-06

# OBS Studio って?

https://obsproject.com/ja

ビデオ録画と生放送用の無料でオープンソースのソフトウェア。    
  
Windows、Mac、Linuxですばやく簡単にダウンロードして配信を開始できます。

とのこと。

![](https://obsproject.com/assets/images/OBSDemoApp2610.png)

バーチャルカメラ配信コンソール的な?

https://note.com/youten_redo/n/ndadc8d3e3c09

これ使ってバーチャルキャラクタで 仮想カメラで Zoom に配信できるそうだ。

REALITY スマフォアプリがバーチャルキャラでライブ配信するものを

ミラーリングして、OBS Studio にある仮想カメラに流し込むそうだ。

### install してみた

![[Pasted image 20211006094732.png]]

仮想カメラのみでいってみる。

ソースでウィンドキャプチャを選択してみた。

![[Pasted image 20211006095105.png]]

仮想カメラ開始

![[Pasted image 20211006095132.png]]

Zoom で試してみる。

カメラを切り替えたらバーチャルカメラに切り替わった。

![[Pasted image 20211006095827.png]]

仮想カメラのサイズを 1280 x 720 と小さくしとくらしい。

![[Pasted image 20211006100232.png]]

水平反転変換はソースを選んで右クリック

![[Pasted image 20211006100544.png]]

### アバター

iPhone で REALITY をインストールした。

デフォで設定されてるキャラ。

![[Pasted image 20211006102324.png]]

iPhone を Mac にケーブルでコネクトすると、Quick Time で新規ムービーで録音開始するとアバターがリアルタイムでキャプチャされる。

![[Pasted image 20211006102626.png]]

これを OBS Studio のソース: 画像キャプチャで Quick Time を選択する。

![[Pasted image 20211006102748.png]]

仮想カメラを開始して、Zoom で仮想カメラに切り替える。

![[Pasted image 20211006103053.png]]
