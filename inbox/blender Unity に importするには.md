---
type: note
---

#blender #unity

---
2022-08-01  13:28

# blender Unity に importするには

FBX 形式にするらしい。

https://tomosoft.jp/design/?p=42022

![[Pasted image 20220801133029.png | 400]]

これを export する。

![[Pasted image 20220801133101.png]]

チェックは入れない。
Object Types で Camera Lamp 以外。

![[Pasted image 20220801133156.png]]

![[Pasted image 20220801133311.png | 200]]

このファイルを  Assets に D&D する。

![[Pasted image 20220801133452.png]]

シーンに D&D できたけど、画像テクスチャが反映されてなかった。

![[Pasted image 20220801134715.png]]

画像テクスチャーを D&D したら、自動的にアサインしてくれた。

![[Pasted image 20220801135023.png]]

がしかし、png のアルファチャンネルに対応してくれてない。


blender のマテリアルで Alpha clip にしてみた。
![[Pasted image 20220801141103.png]]

![[Pasted image 20220801141255.png]]

バックもクリッピングされてるみたいに見える。

もう少しマークだけ前に出すかな。
でもダメだった。

Blender ではアルファクリップと言っている。
Unity では Cutout と呼ばれてるらしい。

わからん。Unity で貼ればいいのかな。

![[Pasted image 20220801155149.png]]

Unity で plane に透過PNG を貼って Rendering Mode で Cutout を選ぶと上手くいくが、これが一緒の部品になって欲しいのかな。

このままでは、デカール貼っても Unity じゃ上手くいかないジャン。


![[Pasted image 20220801155305.png]]


## Import Settings で上手くいった

![[Pasted image 20220801161029.png]]

![[Pasted image 20220801161053.png]]

FBX で import したオブジェクトの Inspector --> Materials で
Location を Use External Materials(Legacy) にしたら上手くレンダリングいった!!
アルファクリップが Unity 上でも上手く行った。


Legacy とあるのが気になる。

https://docs.unity3d.com/ja/2018.4/Manual/FBXImporter-Materials.html

## Material タブ

なんと、Legacy の方は以前との互換で非推奨とのこと。

推奨の Embedded Materials でやってみる。

## Use Embedded Materials

![[Pasted image 20220801162335.png]]

それぞれ下でアサインしたら上手く行った。

![[Pasted image 20220801162420.png]]

なんとここもエディット出来るようになってた。
ちゃんと Cutout 設定できてる。

![[Pasted image 20220801162440.png]]

今まで不活性で動かせなかったのは、Embedded 指定で、内部アサインできてなかったからのようだ。

アサインしてない body とか、 shadow マテリアルは、不活性のままだ。

![[Pasted image 20220801162741.png]]

よくわかりました!!



