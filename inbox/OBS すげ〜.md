---
type: note
---

#obs

---
2022-07-20  09:56

# OBS すげ〜

![[Pasted image 20220720095657.png]]

https://obsproject.com/ja/download

# VTuber になる
VRoid Studio で作った VRM アバターを 3tene で VRoid Studio から直接読み込む。
https://3tene.com/

![[Pasted image 20220720100148.png]]

画像アイコンからグリーンバックにする。

![[Pasted image 20220720100335.png]]

これを OBS に取り込む。

シーン ->ソースで ウィンドウキャプチャを使う。
![[Pasted image 20220720100522.png]]

ソースにフィルタを適用して、エフェクトフィルタからクロマキーを選択して調整する。

![[Pasted image 20220720100648.png]]

これでバックが透過される。
背景画像を同じ要領でセットするとこうなる。

![[Pasted image 20220720101453.png]]

仮想カメラ開始すると Zoom でこのまま移せる。

![[Pasted image 20220720102212.png]]

この設定をシーンコレクションの複製で保存出来る。

![[Pasted image 20220720102544.png]]

シーンコレクションで切り替えることが出来る。

# 待ち画面を作る
シーンコレクションで新規で作る。
![[Pasted image 20220720102814.png]]
![[Pasted image 20220720105600.png]]

Zoom で逆になってる。

![[Pasted image 20220720105943.png]]

実は反転して見えてるのは自分だけらしい。

ビデオ設定で 「ビデオ --> マイビデオをミラーリング」を無効にするのチェックを外すと自分もOKになる。

![[スクリーンショット 2022-07-20 11.02.13.png]]

# ブラウザを表示してアバターも映す

Youtube などを chrome で表示させとく。

ソースでウィンドキャプチャが使いやすいらしい。

![[Pasted image 20220720110656.png]]

ウィンドキャプチャでアバターを映す。
とか映像デバイスでカメラを使ったり。

![[Pasted image 20220720111115.png]]

マスクをフィルターで使って円に切り抜いてみる。

800x800 の画像を用意するといいらしい。

![[Pasted image 20220720112301.png]]

これをフィルターの エフェクトフィルタ-->イメージマスク --> アルファマスクでこうなる。

![[Pasted image 20220720112440.png]]

![[Pasted image 20220720112754.png]]

こんな感じになった。

Blender でお絵描きも出来る。

![[Pasted image 20220720114056.png]]

ただ重すぎて凄い遅延。


## iPhone を Web カメラにしたい

EpocCam Webcam for Mac and PC を使ってみる。

Mac にドライバーをインストールする。
https://www.elgato.com/en/downloads

![[Pasted image 20220720135813.png | 300]]
```
ここで起動してるようだ。

/Library/LaunchAgents/com.kinoni.epoccam.daemon.plist
```

iPhone でアプリを起動すると mac に繋ぎにいく。

OBS に出る様になった。

![[Pasted image 20220720142038.png]]

![[Pasted image 20220720142144.png]]

まんまグリーンバックになった。
無料版だと、ウォーターマークが入る。
有料でもいいかな。

