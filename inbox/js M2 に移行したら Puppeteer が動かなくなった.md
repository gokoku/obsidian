---
type: note
---

#js/puppeteer #m2

---
2023-04-11  09:55

# js M2 に移行したら Puppeteer が動かなくなった

m2 に移行したら Puppeteer が動かなくなった。

Playwright にしようかと思ったら、こっちはテストフレームワーク ビシバシなやつじゃん。
俺はテストがしたいのではなく、ただ単にブラウザを操りたいだけなのだ。

ということで、mac M2 でも Puppeteer を動かすようにした。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://zenn.dev/tnzk/articles/a99b246faf5f69" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://res.cloudinary.com/zenn/image/upload/s--R2j1NOpi--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Apple%2520Silicon%2520%252F%2520M1%2520%25E3%2581%25A7%2520Puppeteer%2520%25E3%2581%25AE%25E3%2582%25A4%25E3%2583%25B3%25E3%2582%25B9%25E3%2583%2588%25E3%2583%25BC%25E3%2583%25AB%25E3%2581%25AB%25E5%25A4%25B1%25E6%2595%2597%25E3%2581%2599%25E3%2582%258B%25E5%25A0%25B4%25E5%2590%2588%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:%25E3%2581%25AF%25E3%2581%25BE%25E3%2581%2590%25E3%2581%25A1%2520%252F%2520kyohei%2Cx_203%2Cy_98/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2IxYmIxOTFkYWQuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_72/og-base.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Apple Silicon / M1 で Puppeteer のインストールに失敗する場合</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://zenn.dev/tnzk/articles/a99b246faf5f69
		</p>
	</div>
</a></div>

まだいけるじゃん。

```shell
$ brew install cromium
```

bin/puppeteer/.env に追記。
```shell
export PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true
export PUPPETEER_EXECUTABLE_PATH='/opt/homebrew/bin/chromium'
```


obc.js
```js
const browser = await puppeteer.launch({
	headless: false,
	executablePath: process.env['PUPPETEER_EXECUTABLE_PATH'],
	....
```

ここで executablePath を `which chromium` にするとまだ行ける!!


puppeteer を update するのに npm-check-updates を使う。

[[js     古いパッケージを知る]]


