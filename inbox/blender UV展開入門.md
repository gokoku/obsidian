---
type: note
---

#blender 

---
2022-07-27  09:34

# blender UV展開入門

https://comic.smiles55.jp/guide/10269/

シームを入れる。

辺を選択して、右クリックで Mark Seam

![[Pasted image 20220727093629.png]]


### UV 展開

マテリアルから Select で選択する。

UV Editing で UV画面で右クリックして Unwrap (展開)する。

U キーでもメニューが出る。

![[Pasted image 20220727093831.png]]


![[Pasted image 20220727100122.png]]
連動をクリックすると、どこが選択されているかがわかる。
これが UVマッピング。


## イメージとリンクさせる

新規イメージの追加。

![[Pasted image 20220727100725.png | 300]]
名前つけて OK。

Nキーでシェルフを出すと、カラーが変えられる。

![[Pasted image 20220727101213.png || 300]]

イメージを保存する。

![[Pasted image 20220727101909.png]]

ここではまだマテリアルとイメージがリンクしてないので、反映されてない。
![[Pasted image 20220727102243.png | 300]]

## テクスチャの作成

マテリアルの Base Color のポチっをクリックするとメニューが出る。

![[スクリーンショット 2022-07-27 10.24.48.png]]

Image Texture を選ぶとこうなる。

![[Pasted image 20220727102658.png]]

ここでイメージマークをクリックすると、イメージ一覧が出るので、選択する。

![[Pasted image 20220727102758.png]]


![[Pasted image 20220727102847.png]]

適応された。


これでペイントの準備が出来たらしい。

## ペイント

![[Pasted image 20220727103210.png | 400]]

ワークスペースを Texture Paint にする。
モードは Image Editor

UV --> export UV Layout で展開図。
Image --> save でイメージファイル。

レイヤーに重ねてフォトショみたいなので描く。


## Blender ファイルに入れる

image --> Pack で外部イメージファイルが取り込まれる。


![[Pasted image 20220727110859.png]]


これでお絵描きが出来るってことか。
