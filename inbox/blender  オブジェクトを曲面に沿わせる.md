#blender 


Shringwrap Modifier を使うらしい。

https://www.youtube.com/watch?v=qrlfmcQ2lCs&t=1s



https://twitter.com/M_seconda_volta/status/1243409155567702018


自分なりにやってみる。

![](image-komjt7rx.png)

## cube を玉に沿わせる。

### プレートを用意

Plate を作って、このプレートを cube にバインドさせて、プレートを玉に沿わせる。

1![](image-komk4quh.png)

プレートを Viewport Display でワイヤーにして、

![](image-komk70ul.png)

Modifiere で Solidify で厚みをつけてラティス状にする。
Apply を忘れずにやること。

![](image-komk82w0.png)

これでキューブを包んでやらないとダメなようだ。

### キューブの設定

Modifirer で MeshDeform を設定して、オブジェクトにプレートを設定して Bind をクリックする。

![](image-komkbn26.png)

### プレートの設定
プレートでは、Modifier で Shrinkwrap を設定して、Target を玉にする。

![](image-komkfs6u.png)

順番は、Shrinkwrap をプレートに適応してから、立体にするの順。


![](image-komkgspv.png)

Offset で引っ込めたり、出したりできる。

Modifier を Apply してプレートを削除する。

# Weight を使ってやる方法

https://note.com/info_/n/n60939bf2db2e

こちらは吸着だ。

スナップで沿った移動が可能だ。


# キューブ

細分化する。

![](image-komkrm2z.png)

All select して、頂点グループに登録する。
Ctrl + G

![](image-komktiyd.png)


### Weight mode

頂点選択にして、

![](image-komla6ht.png)

Box 選択する。
![](image-komlaykb.png)

ブラシを選択すると、ツールシェルフが weight 設定できる。
![](image-komlbbl2.png)

tool シェルフで Weight を設定する。

![](image-komldgys.png)

ここで shift + k をすると、選択した頂点にこの weight が設定される。

![](image-komlfa9f.png)

下を weight 1.0 上を 0.85 

![](image-komlhxpc.png)

## modifier

Modifier の Shrinkwrap をキューブに設定する。
Target を玉、頂点グループを登録したグループにする。

![](image-komljuo9.png)


張り付く!!
![](image-komlog5b.png)

動かせる。

![](image-komlqqfk.png)