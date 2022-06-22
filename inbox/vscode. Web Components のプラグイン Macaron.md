---
type: note
---

#vscode #web 

---
2022-06-17  13:16

# vscode. Web Components のプラグイン Macaron


<div class="rich-link-card-container"><a class="rich-link-card" href="https://macaron-elements.com/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://macaron-elements.com/ogp.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Macaron | Macaron</h1>
		<p class="rich-link-card-description">
		Open-source visual editor to create and maintain Web Components
		</p>
		<p class="rich-link-href">
		https://macaron-elements.com/
		</p>
	</div>
</a></div>

何だこりゃ。web compoent を GUI で編集できて、挙げ句の果てにそれを使えるようになるらしい。

早速 vscode に入れた。

```shell
$ npm init vite@latest
macaron-sample

$ cd macaron-sample
$ npm i
```

`omponents.macaron` ファイルを作った。

![[Pasted image 20220617132035.png]]

こうなる。

![[スクリーンショット 2022-06-17 13.22.42.png]]
 Text を click すると my-component が出来た。


コンパイルして読み込むのは上手くいったが、vite で動かなかった。
