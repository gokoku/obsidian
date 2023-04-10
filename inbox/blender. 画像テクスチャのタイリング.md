node---
type: note
---

#blender

---
2022-09-14  16:58

# blender. 画像テクスチャのタイリング

https://yuki3dcg.com/3dcg/post-436/

コンクリートの素材が欲しい。

https://kenchiku-pers.com/download/86.html

建築パース.com って何?

![[ｺﾝｸﾘｰﾄO001.jpg]]

こういうやつDLした。

これをタイリングで倍率を自由に帰るというわけですね。

マテリアルにこの画像を Base color で選択した。

![[Pasted image 20220914171625.png]]

一枚の絵がベタんと貼られただけ。

テクスチャ座標 (input -->Texture Coordinate) と、マッピング( Vector --> Mapping) を追加。

![[Pasted image 20220914172139.png | 900]]

Mapping の Scale を上げるとタイリング幅がコントロールできる。

![[Pasted image 20220914172235.png]]


これを一発でやってくれる add-on があるとのこと。

![[Pasted image 20220914172421.png]]

Node Wrangler    wrangler : 口論するらしい


画像の node 選んで Ctrl + T をすると、一発。

![[Pasted image 20220914172916.png]]

配線までしてくれる。


## Node Wrangler

使い方。

https://yuki3dcg.com/3dcg/blender-node-wrangler/

### 複数テクスチャの自動読み込み

### テクスチャセット作成
これが上のやつらしい。

### テクスチャ画像のリロード

Atl + R


色々ありすぎてもういいや。

