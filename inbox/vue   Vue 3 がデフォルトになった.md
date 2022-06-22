#js/vue 

---
2022-03-03  16:21

# vue   Vue 3 がデフォルトになった


<div class="rich-link-card-container"><a class="rich-link-card" href="https://codezine.jp/article/detail/15628" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://codezine.jp/static/images/article/15628/15628_og.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Vue.jsの世代交代が到来！ Vue 3デフォルト時代の「Vue.js開発新常識」</h1>
		<p class="rich-link-card-description">
		本連載では、JavaScriptフレームワーク「Vue.js」を、型定義が利用できるようJavaScriptを拡張した言語「TypeScript」で活用する方法を、順を追って説明していきます。前回はVue.jsでコーディングするための基本的な記法を説明しました。今回はVue.jsの公式ブログから発信された「Vue 3をデフォルトバージョンにする」発表と、それに伴って、これまでと変わっていくVue.js開発の新常識を紹介していきます。
		</p>
		<p class="rich-link-href">
		https://codezine.jp/article/detail/15628
		</p>
	</div>
</a></div>

Vue.js作者のEvan You氏は、2022年1月20日に公開された[ブログ記事](https://blog.vuejs.org/posts/vue-3-as-the-new-default.html "ブログ記事")で、2022年2月7日以降はVue 3がデフォルトバージョンになると宣言しました。

とのこと。

```shell
$ npm init vue@latest
$ cd sample
$ npm i
$ npm run lint
$ npm run dev
```

http://localhost:3000

![[Pasted image 20220303162437.png]]

![[Pasted image 20220303162519.png]]

### トレンド

CLI ツールが Vite ベース。

Composition API 記述。

VSCode の機能拡張が Volar 。
![[Pasted image 20220303164603.png]]

Vuex よりシンプルな Pinia が CLI で導入されるらしい。

![[Pasted image 20220303164920.png]]

