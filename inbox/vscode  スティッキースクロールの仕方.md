---
type: note
---

#vscode 

---
2022-08-19  13:20

# vscode  スティッキースクロールの仕方


<div class="rich-link-card-container"><a class="rich-link-card" href="https://coliss.com/articles/build-websites/operation/work/sticky-scroll-for-vs-code.html" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://coliss.com/wp-content/uploads-202203/2022080701.gif')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">VS Codeの新機能がすごく便利！ JavaScriptやCSSの関数やクラスなどを上部に固定させる「スティッキー スクロール」</h1>
		<p class="rich-link-card-description">
		8/5にアップデートされた、VS Code v1.70で「スティッキー スクロール」を使用できるようになったので、紹介します。 JavaScriptやCSSで作業しているときに、関数やクラスなどが自動
		</p>
		<p class="rich-link-href">
		https://coliss.com/articles/build-websites/operation/work/sticky-scroll-for-vs-code.html
		</p>
	</div>
</a></div>


preference --> search で editor > experimental > sticky 
Sticky Scroll をチェック入れる。



![[Pasted image 20220819132329.png]]


![[Pasted image 20220819132459.png]]

確かにメソッドで引っ付くようになった。
結構便利かも。

色つけ

setting.json

```json
    "workbench.colorCustomizations": {
        "editorStickyScroll.background": "#332765",
        "editorStickyScrollHover.background": "#504083",
    }
```

