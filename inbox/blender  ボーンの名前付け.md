#blender 


http://cignoir.hatenablog.jp/entry/2014/05/19/101552

名前を統一したい。

![](image-km2qe0e8.png)

Unity の Humanoid に対応しているやつらしい。

オブジェクトの中のボーンがみたい。
View port shading を solid にすると見える。

ボーンはアーマチュアを選択して、Edit mode にすると組めるようになる。
E で extend して追加していく。

## 名称はプロパティでつける

ボーンを選択した状態でプロパティに名称が出るので
![](image-km2uvcjb.png)

## 対象化

左側の名前に .L を付ける。


ボーンを All select して、右クリックからの Symmetries


## ウェイトを付ける

オブジェクトmode にする。

オブジェクトを選択してから、

ボーンを選択して

右クリックから parent --> With Automatic Weights

![](image-km2vjpyj.png)

### 確認する

ポーズモードにして、IK の場合は回転だけでいじるらしい。

## ウェイトペイント

自動ウェイトがめちゃクチャだった。
Tポーズが基本ということか。

ボーンを選択して、

オブジェクトを選択して、

ペイントモードにする。

![](image-km2vzduf.png)

ボーンの選択は
```
Ctrl + 左クリック

Shift + 左クリックでも


Edit --> Lock Object Modes のチェックを外す。
Post mode にしてからメッシュ(オブジェクト)を選択する。

```
