---
type: note
---

#raspberry_pi #node-red

---
2023-01-25  17:51

# raspberry_pi Node-red で jQuery 使いたい

![[Pasted image 20230125181903.png]]

uibuilder インスタンスから Libraries でインストール出来るっぽい。

![[Pasted image 20230125182004.png]]

![[Pasted image 20230125182022.png]]

入ったんじゃね?

## html に設定する

http://people10.local:1880/uibuilder/uibindex
こっちに説明があった。

ここで入れたパッケージのパスを通してやるといいとある。
パスは node_modules までのパスは ../uibuilder/vendor で設定してあるからその先を書くらしい。
index.html
```html
<script src="../uibuilder/vendor/jquery/dist/jquery.js"></script>
```

index.js
```js
console.log( $(document))
```

やった!!!
通りました!!!
これで jquery を使える!!!
