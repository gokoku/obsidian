---
type: note
---

#rails

---
2022-11-16  12:00

# rails  jsPDF ライブラリを使いたい

```shell
$ yarn add jspdf

$ bin/importmap pin jspdf
```

pin ファイルは config/importmap.rb に記述される。
```rb
pin "jspdf", to: "https://ga.jspm.io/npm:jspdf@2.5.1/dist/jspdf.es.min.js"
```
これでいいのかわからん。
node_modules のパスで指定しても失敗する。`node_modules/jspdf/dist/jspdf.es.min.js`


javascript/application.js に記述する。

```js
import jsPDF from 'jspdf';
window.jsPDF = jsPDF;
```
jquery と同じように window オブジェクトにぶら下げてみた。
```js
import jquery from "jquery";
window.jQuery = jquery
window.$ = jquery
```

すると、<script> タグで使えるようになった。

_template-a01.htm.erb_

```js
<script>
const downloadPdf = () => {
  const img = canvas.toDataURL('image/png', 1.0)
  const doc = new jsPDF()
  doc.addImage(img, 'png', 0, 0, 297, 210)
  doc.save('download.pdf')
  console.log("pdf download")
}
</script>
```
