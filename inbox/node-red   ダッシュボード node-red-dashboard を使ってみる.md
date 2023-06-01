#node-red #raspberry_pi 

---
2021-12-10

# ダッシュボード node-red-dashboard を使ってみる

https://qiita.com/takeyan/items/fff5e8d64da25220a0c0

温度をMQTTで受信してるところ

![[Pasted image 20211210142259.png]]

[[node-red  MQTT でデータを受信]]

パレット管理から node-red-dashboard をインストールする。

![[Pasted image 20211210151815.png]]

## 温度をテキストで表示してみる

![[Pasted image 20211210151912.png]]

因みにここから dashbord タブを開ける。

![[Pasted image 20211210152014.png]]

text node 編集で、Group を指定する。
新規で設定する時、鉛筆マークで名前を決める。

![[Pasted image 20211210152155.png]]

Default から IoT Sensor とか書く。

![[Pasted image 20211210152405.png]]

次に Tab を指定する。これも新規の時は、鉛筆マークから書き込む。

![[Pasted image 20211210152535.png]]

これで、ノードの赤い三角は消えて、右のタブに今追加したものが表示されている。

![[Pasted image 20211210152652.png]]

結線して Deploy する。

![[Pasted image 20211210152734.png]]

右の開くマーク? をクリックする。

![[Pasted image 20211210153026.png]]

xxxxx.local:1880/ui で今の値がリアルタイムで表示された。

![[Pasted image 20211210153149.png]]




## 温度をメーターで表示してみる

同じように gauge をセットする。最大値を50にする。

![[Pasted image 20211210153552.png]]

![[Pasted image 20211210153505.png]]

ゲージが出た。

![[Pasted image 20211210153626.png]]

## レイアウト

グループを追加してまとめてみるとあっという間にこうなった。

![[Pasted image 20211210161119.png]]

![[Pasted image 20211210161138.png]]

![[Pasted image 20211210161222.png]]

