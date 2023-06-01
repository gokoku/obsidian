---
type: note
---

#js

---
2022-06-09  09:54

# js Web Components とは?


<div class="rich-link-card-container"><a class="rich-link-card" href="https://codezine.jp/article/detail/15921" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://codezine.jp/static/images/article/15921/15921_ogp.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">進化し続けるフロントエンド技術「Web Components」はライブラリ不要！――気軽に始めて開発の生産性を高めよう</h1>
		<p class="rich-link-card-description">
		再利用可能なUI部品を組み合わせて画面を構築していくのは、多くのGUIアプリケーションにとって効率の良い開発手法です。ブラウザ向けにも多くのライブラリが再利用性を担保するための工夫を重ねてきましたが、実はブラウザ自身が再利用可能なUI部品を作成するための機能を備えていることはご存知でしょうか。本記事では、Web Componentsという名称で総称されるブラウザの機能について解説します。
		</p>
		<p class="rich-link-href">
		https://codezine.jp/article/detail/15921
		</p>
	</div>
</a></div>


main.js

```js
class MyGreatBeautifulButton extends HTMLElement {
  constructor() {
	super()
	this.attachShadow({ mode: 'open' })
	const backgroundColorAttr = this.getAttribute('backgroundColor') || '#eee'
	const colorAttr = this.getAttribute('color') || '#000'

	this.shadowRoot.innerHTML = `
	  <style>
		button {
		  border: none;
		  border-radius: 8px;
		  padding: 10px 20px;
		  background-color: ${backgroundColorAttr};
		  color: ${colorAttr}
		}
	  </style>
	  <button type="submit">
	    <slot name="label"></slot>
	  </button>
	`;
  }
}
customElements.define('')
```