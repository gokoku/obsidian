---
type: note
---

#blender

---
2022-07-29  10:53

# blender UV テクスチャーを Photoshop で描く

全選択して、UV --> Export UV Layout

![[Pasted image 20220729105356.png]]

これと、テクスチャイメージを重ねて Photothop

![[Pasted image 20220729105526.png]]

これでテクスチャーイメージを描く。

レイヤーだけを クイックpng書き出しで書ける。

Blender では Image で Reload すると反映される。

![[Pasted image 20220729112402.png]]


![[Pasted image 20220729112440.png]]

こんな感じでできるようになったよ。

## シールのように貼りたい

alpha チャンネル使ってボディはマテリアル、マークをデカールのように貼りたい。

alpha 領域は画像の大きさに入らないようで、Blender でずれて困った。

(0.0) は色載せておけば領域が確保されてずれなくなった。

![[Pasted image 20220729161853.png || 300]]

が、Display 用の画像をテクスチャで貼って、Apple Logo は別ファイルで次のアドオンで貼ることにした。



## add - on で凄いのがある

image-Export:Import Images as Planes 

デフォルトで入ってるので、チェック入れるだけで、出来てしまうようだ。

https://cgbox.jp/blender-alpha/

object mode で shift + A --> Image --> Image as Planes

![[Pasted image 20220729172536.png]]

画像を選ぶと、

![[Pasted image 20220729172718.png | 100]]

Node では配線済みになってる、alpha が一緒につながってる。

![[Pasted image 20220729172902.png | 500]]

あれ?
これだけか。
あとはオブジェクトに沿ってぴたりと貼るのか。

一緒だった。

Eevee では透過にならないから、その設定もされてるようだ。

マテリアル --> ![[Pasted image 20220729173423.png]]

settings --> Blend Mode : Alpha Blend
               --> Shadow Mode : Opaque   ( Alpha Hash でも Alpha Clip でもいいらしい )



## Plate を貼る

モディファイヤーからシュリンクラップを選ぶらしい。

![[Pasted image 20220729174317.png]]

貼る奴を選択して、Modifire --> Shirinkwrap を選ぶ。

![[Pasted image 20220729174406.png]]

Target で貼られるやつをスポイトで選ぶと、

![[Pasted image 20220729174455.png]]

これでいける。
もういいや。



