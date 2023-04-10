---
type: note
---

#cocoar #blender

---
2022-07-28  10:17

# cocoar で png テクスチャ 3D上げられた

![[Pasted image 20220728101942.png]]

UVテクスチャ貼った 3Dオブジェクトを cocoar に上げられるかやってみた。

フォルダーに obj ファイルと mtl ファイルと 貼った png ファイルを一緒に入れた。

![[Pasted image 20220728102211.png]]

これで zip ファイルを作った。

```shell
$ rm apple/.DS_Store
$ zip -r apple.zip apple
```

![[Image-1 3.jpg]]

貼った画像テクスチャも含められることがわかった。


## Object形式の export 

Y Forward   Z up で export すると天地が揃う。