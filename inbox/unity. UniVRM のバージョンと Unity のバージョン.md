
---
type: note
---

#unity #cluster 

---
2022-08-02  20:10

# unity. UniVRM のバージョンと Unity のバージョン

https://clusterhelp.zendesk.com/hc/ja/articles/360029465811-%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%A0%E3%82%A2%E3%83%90%E3%82%BF%E3%83%BC%E3%81%AE%E5%88%B6%E9%99%90

cluster のアバターのバージョン

[UniVRM v0.61.1](https://github.com/vrm-c/UniVRM/releases/tag/v0.61.1)

![[Pasted image 20220802201321.png]]

[UniVRM-0.61.1_7c03.unitypackage](https://github.com/vrm-c/UniVRM/releases/download/v0.61.1/UniVRM-0.61.1_7c03.unitypackage)

こちらダウンロード。



これの Unity は -   推奨バージョン Unity-2019.4 LTS (Recommended)

![[Pasted image 20220802202118.png]]

モジュールは特にいらないらしい。日本語だけにした。

新規プロジェクト
![[Pasted image 20220802202312.png]]

2019.4.40 を選ぶこと。
3D で作る。


VRM をダブルクリックすると import 出来る。


FBX を読み込むフォルダー作成。

![[Pasted image 20220803085411.png]]


この中に FBX とマテリアルを入れる。

![[Pasted image 20220803085540.png]]

FBX の inspector で Materials でクリックする。
このままということで閉じると、マテリアルがアセットに出来てる。

![[スクリーンショット 2022-08-03 9.00.14.png]]


# ユニティーちゃんトゥーンシェーダー UTS

https://unity-chan.com/download/releaseNote.php?id=UTS2_0

これを import する。

テクスチャと同じ名前の mat ファイルを選択する。
Shader から UnitiChanToonShader/Toon_DoubleShadeWithFeather を選ぶ。

![[Pasted image 20220808173745.png]]


1st Shade Map と 2nd ShadeMap に テクスチャを D&D したところ。

![[Pasted image 20220808174045.png]]

影の調整をする。

![[Pasted image 20220808174546.png]]







マテリアルの inspector でshader  で VRM --> MToon を選択する

![[Pasted image 20220803091152.png]]


![[Pasted image 20220803091525.png]]

テクスチャでカラーをいじろうとすると、mac に止められる。


![[Pasted image 20220803091508.png]]

カラーを白にしてやると、明るくなる。

![[Pasted image 20220803091632.png]]

![[Pasted image 20220803091714.png]]


リグを設定する。

![[Pasted image 20220803091856.png]]

FBX で Humanoid で apply

この後、Config クリックして save する。

![[Pasted image 20220803092026.png || 200]]

こんなの出てきた。

![[Pasted image 20220803092102.png | 200]]

無訂正でいけたので、Done
訂正したら apply --> Done


修正入るやつは空になってるところにボーンを D&D してやる。

VRM は Upper Chest は None で OK とのこと。



### VRM の書き出し

![[Pasted image 20220803092634.png]]

Avater を選択してメニューの VRM --> UniVRM-0.61.1 --> Export humanoid

![[Pasted image 20220803093203.png | 500]]
大体入れたら Export

![[Pasted image 20220803093300.png]]

出来てた。

# リップシンクをつけたバージョン

新規フォルダを用意して、ここに VRM ファイルを D&D

![[Pasted image 20220803093444.png | 200]]

ドバッとファイルが入ってきた。
この中の FBX を Hierarchy に D&D して表示。

BlenderShapes を選択する。

![[Pasted image 20220803093932.png]]

この中の BlenderShape の Inspector をみると、

![[Pasted image 20220803094048.png]]

inspector の下から絵を出す。

ホイールで拡大縮小できる。

![[Pasted image 20220803094238.png]]

A を選んで、Shape をアサインしてくようだ。

![[Pasted image 20220803094447.png]]

アイウエオしか用意してないので、他は端折る。

リップシンクを設定し終わったら、Hierarchyで選択して、メニュー --> VRM --> UniVRM-0.61.1 --> Export humanoid

# cluster にアップロードする

https://cluster.mu/account/avatar

 ここでアバターをアップロードする。

Neck がないと言われて失敗した。

![[Pasted image 20220803095736.png]]

Hierarchy で Neck を D&D で当てた。

Apply + Done

一旦 VRM で Export しないと、BlendShape が出ないな。

同じ要領でリップシンクしたのを VRM export した。

アップ出来た!

![[Pasted image 20220803100934.png]]

![[Pasted image 20220803101344.png]]

行けました。



# Blender から FBX にシェイプキーが転送されない

![[Pasted image 20220809103229.png]]

まず、ここの Mirror を外すらしい。
モディファイのミラーは適応済みにする。


FBX の設定で、Geometry の Apply Modifiers をチェック外す。

![[Pasted image 20220809103606.png]]

Apply Modifiers を外すと、カクカクのポリゴンになった。

Shader に MToon を適応させて見られるようになった。

VRM ファイルを D&D して、BlendShapes の中の BlendShape をクリックしたらShape 設定が出た。

![[Pasted image 20220809104559.png | 300]]

これで設定していく。


カクカクのポリゴンは Property の Normals --> Auto Smooth にチェックを入れて角度を調整する。

けど、チェック外したほうがスムーズになった。

![[Pasted image 20220809144917.png | 300]]

これでいいんじゃね??

VRM をこれで作る。

cluster にあげた。
![[Pasted image 20220809151146.png]]


![[スクリーンショット 2022-08-09 14.38.33.png]]