#css #js

---
2022-04-05  16:11

# css  Canvas要素をA4サイズにマッピングしたい

canvas 要素を A4 実寸と合わせたい。

```shell
$ npm init vite@latest
project name : pdf-canvas
               typescript
               
$ cd pdf-canvas
$ npm i
$ npm run dev
```

http://localhost:3000/


72dpi では pdf がぼやぼやなので、300dpi にする。
A4 の 300 dpi で 210mm-->2480px
                             297mm-->3508px

タグでの width と height の指定が基準になるようだ。
ここでは px のみ。

```html:index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
  </head>
  <body>
    <canvas class="pdf-content" size="A4" width="2480" height="3508" >A4</canvas>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>
```

css では mm で大きさが指定出来るようだ。

```css:style.css
body {
  background: rgb(204,204,204);
}
canvas.pdf-content {
  display: block;
  background: white;
  margin: 0 auto;
  box-shadow: 0 0 0.5cm rgba(0,0,0,0.5);
}
canvas.pdf-content[size="A4"] {
  width: 210mm;
  height: 297mm;
}
canvas.pdf-content[size="A4"][layout="portrait"] {
  width: 297mm;
  height: 210mm;
}
@media print {
  body, div {
    margin: 0;
    box-shadow: 0;
  }
}
```

72 dpi から 300 dpi にすることにしたので、mm2px は 11.81倍
少数以下は二桁必要のようだ。

```ts:maints
import './style.css'


const canvas = document.querySelector('.pdf-content') as HTMLCanvasElement
const ctx = canvas.getContext('2d')!

const mm2px = (mm: number) => mm * 11.81

ctx.fillStyle = "rgb(200, 0, 0)"
ctx.fillRect(0, mm2px(50), mm2px(210), mm2px(50))

ctx.fillStyle = "rgba(0, 0, 200, 0.5)"
ctx.fillRect(mm2px(30), mm2px(30), mm2px(50), mm2px(50))

ctx.arc(mm2px(100), mm2px(100), mm2px(50), 0, Math.PI * 2, true)
ctx.fillStyle = "rgb(20, 200, 0)"
ctx.fill()
```

![[Pasted image 20220405161818.png]]

PDF に変換する。

```js:pdf.js

'use strict';
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('http://localhost:3000', {waitUntil: 'networkidle2'});
  await page.pdf({path: 'hn.pdf', format: 'A4'});

  await browser.close();
})();
```

```shell
$ node pdf.js
```

pdf を実寸でプリントすると、同じ寸法だった。


## What next ?
canvas が body と同じなので、puppeteer でまんま変換しても A4 サイズピッタリだった。

ヘッダとかツールとか他の要素があるので、canvasだけをPDF化したい。

### jsPDF を使うとできた

一旦画像ファイルにしてPDFに貼り付けるやり方で出来る。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://stackoverflow.com/questions/19065562/add-image-in-pdf-using-jspdf" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://cdn.sstatic.net/Sites/stackoverflow/Img/apple-touch-icon@2.png?v=73d79a89bded')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Add image in pdf using jspdf</h1>
		<p class="rich-link-card-description">
		I am using jspdf to convert an image into a PDF.

I have converted the image into a URI using base64encode. But the problem is that there are no errors or warnings shown in the console. A PDF is
		</p>
		<p class="rich-link-href">
		https://stackoverflow.com/questions/19065562/add-image-in-pdf-using-jspdf
		</p>
	</div>
</a></div>

```shell
$ npm i jspdf
```


```html:index.html
  <body>
    <canvas class="pdf-content" size="A4" width="2480" height="3508" >A4</canvas>
    <input id="download" type="button" value="Download"/>
    <script type="module" src="/src/main.ts"></script>
  </body>
```
ボタン追加
![[Pasted image 20220405165248.png]]

```js:main.ts

const download = document.querySelector('#download') as HTMLButtonElement
download.addEventListener('click', () => {
  // only jpeg is supported by jsPDF
  var imgData = canvas.toDataURL("image/png", 1.0);
  var pdf = new jsPDF();

  pdf.addImage(imgData, 'png', 0, 0, 210, 297);
  pdf.save("download.pdf");
})

```

PDFファイルをダウンロードする。

![[Pasted image 20220405165337.png]]

バッチリだよ!!

### 文字の出力
文字の pt も変換する必要があった。
pt と pixel は同じらしい。
なので、72dpi から 300dpi にする変換が必要。

```js:main.ts
const mm2px = (mm: number) => mm * 11.81
const px2px = (px: number) => px * 4.17 //72dpi から 300dpi への変換

for(let i = 6; i < 60; i=i+4) {
  ctx.font = `${px2px(i)}px serif`
  ctx.fillText(`${i}pt 幼稚園`, mm2px(10), mm2px(i*5))
}

```
![[Pasted image 20220405180109.png]]

6pt もいける。

![[Pasted image 20220405180135.png]]

