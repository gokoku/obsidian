---
type: note
---

#ai

---
2022-08-30  13:10

# ai.  画像を高解像度にするツール Upscayl

https://github.com/upscayl/upscayl

![[Pasted image 20220830131142.png]]

でもエラーで動かず。

```shell
$ git clone https://github.com/TGS963/upscayl
$ cd upscayl
$ npm install
```

ここで、不具合の解消をする。

```shell
$ cd /Users/george/Desktop/upscayl/resources/mac/bin
$ chmod u+x upscayl
```

戻って起動する。

```shell
$ cd ~/..../upscayl
$ npm run start
```
ドラッグ&ドロップするとエラー出るが、今度はうまく行く。

![[Pasted image 20220830135318.png |500]]

npm で run すると、動いてる様子がわかってとても便利だ。

![[Pasted image 20220830135442.png | 400]]

# アプリも直してみるか

```shell
$ cd /Applications/Upscayl.app/Contents/Resources/bin
$ chmod u+x upscayl
```

バッチリ動くようになった。

![[Pasted image 20220830135829.png | 400]]

