#css

---
2022-04-04  18:12

# css  A4 の大きさを指定出来る

https://stackoverflow.com/questions/37196261/html-what-is-the-page-tag-doing

css でサイズを指定出来るようだ。


```html:index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="print_only" size="A4">A4</div>
  <!-- <div class="print_only" size="A4" layout="portrait">A4 portrait</div>-->
</body>
</html>
```

```css:style.css
body {
  background: rgb(204,204,204);
}
div.print_only {
  background: white;
  display: block;
  margin: 0 auto;
  margin-bottom: 0.5cm;
  box-shadow: 0 0 0.5cm rgba(0,0,0,0.5);
}
div.print_only[size="A4"] {
  width: 210mm;
  height: 297mm;
}
div.print_only[size="A4"][layout="portrait"] {
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

![[Pasted image 20220404181527.png]]

これを PDF に出来るのが puppeteer だ。

```js:pdf.js
'use strict';
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('http://127.0.0.1:5500/index.html', {waitUntil: 'networkidle2'});
  await page.pdf({path: 'hn.pdf', format: 'A4'});

  await browser.close();
})();
```

```shell
$ node pdf.js
```

pdf ファイルが出来てる。
けど、2p分出てる。

![[Pasted image 20220404181816.png]]

部分は A4 に出来るけど、puppeteer ではページ全体で受け取ってるからか。

部分だけ PDF に出来ないかな。

