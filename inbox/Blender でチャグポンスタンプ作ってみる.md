---
type: note
---

#blender

---
2022-05-17  18:40

# Blender でチャグポンスタンプ作ってみる

イラレでアウトラインのパスデータを作った。
黒の単一のオブジェクト。

![[20160512ちゃぐぽん_メイン.2.svg | 300]]
SVG データで保存する。

Blender で import する。

![[Pasted image 20220517184353.png]]

パスからメッシュに変換する。

Object -> Convert -> Mesh

![[Pasted image 20220517184443.png]]

Add Modifier -> Decimade で頂点を少なくする。

![[Pasted image 20220517184519.png]]

Ratio で調節するが、編集モードの時は画面が変わらない。
調節するときはオブジェクトモードにする。

Add Modifier -> Solidify で厚みをつける。

![[Pasted image 20220517184859.png]]




Cylinder で台を作って、マテリアルを SVGマットとやらにする。
SVG に付いてきたマテリアル。

![[Pasted image 20220517185046.png]]

オブジェクトモードで **Ctrl-A で拡大縮小を Apply**

![[Pasted image 20220517185215.png | 500]]

大体 0.1m 直径 2cm の厚み。

これをプリンターで出す。

確か、縮尺が変だったので変えるはず。

![[blender  イノベの3Dプリンターで出す]]


では、実物の10倍のデータにする。

