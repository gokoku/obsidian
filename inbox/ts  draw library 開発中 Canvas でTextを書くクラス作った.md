#ts 

---
2022-04-07  16:27

# ts draw library 開発中 Canvas でTextを書くクラス作った

draw library みたいなことになってきた。
vite で Vanilla typescript で開発中。


![[Pasted image 20220407162805.png]]![[Pasted image 20220407162902.png]]

テキストは改行が対応してないようだ。
なので、ゴニョゴニョして
幅と高さを設定するとその中に収まるように表示出来るようにした。

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
    <input id="download" type="button" value="Download"/>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>
```

CSS とタグのpixel設定で 300dpi を表現している。

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
  /*width: 2480px;*/
  /*height: 3508px;*/
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

Text オブジェクトは DrawObject を親に持つようにした。

```ts:DrawObject.ts
import { Color } from './Color'

export abstract class DrawObject {
  context: CanvasRenderingContext2D

  constructor(context: CanvasRenderingContext2D,
    public x: number = 0,
    public y: number = 0,
    public width: number = 0,
    public height: number = 0,
    public color: Color = new Color(0, 0, 0, 1)
  ) {
    this.context = context
  }

  abstract draw(): void
  abstract dragable(x: number, y: number): boolean
  abstract drag(x: number, y: number): void
}
```

Text オブジェクトを使うときは mm 指定で、フォントは pt 指定で出来るように変換するツールを用意。

```ts:Utility.ts
export const mm2px = (mm: number) => Math.round(mm * 11.81 * 100) / 100 //mm to px
export const px2px = (pt: number) => Math.round(pt * 4.17 * 100) / 100 //72dpi から 300dpi への変換
```

mm2px はまんま mm --> px 変換ただし 300dpi時
pt2px は 72dpi でのフォントptから 300dpi の px に変換する。
小数点2桁とした。

Text クラスは基本情報のx,y,w,h,color とは別にしてフォントサイズptと文字をセットするようにした。

表示の時だけ複数行にする処理をする。

行間を設定した。2022/04/08

```ts:Text.ts
//
// Text
// size, x, y, width はmm単位で指定して、
// ここでpx変換してから描画する(72dpi から 300dpi への変換)

import { pt2px } from './Utility'
import { Property, DrawObject } from './DrawObject'


export class Text extends DrawObject {
  public size: number = pt2px(12)
  public text: string = ''
  public lineHeight: number = 1.3
  constructor(context: CanvasRenderingContext2D, property: Property) {
    super(context, property)
    this.setText(property.option.size, property.option.text)
  }

  setText(size: number, text: string) {
    this.size = pt2px(size)
    this.text = text
  }

  draw() {
    this.context.fillStyle = this.color.toString()
    this.context.font = `${this.size}px Arial`
    // 幅で複数行に分ける
    const hNumber = Math.floor(this.width / this.size)
    let newText = ''
    for(let i=0; i<this.text.length; i++) {
      if( i % hNumber == 0) {
        newText = `${newText}\n${this.text[i]}`
      } else {
        newText = `${newText}${this.text[i]}`
      }
    }
    const lines = newText.split('\n')
    lines.forEach((line, i) => {
      // 高さで切る
      if( i*this.size*this.lineHeight >= this.height ) {
        return
      }
      this.context.fillText(line, this.x, this.y+i*this.size*this.lineHeight, this.width)
    })
  }

  dragable(x: number, y: number): boolean {
    return true
  }

  drag(x: number, y: number) {
  }
}
```

これを使って描画する。

```ts:main.ts
import './style.css'
import jsPDF from 'jspdf'

import { mm2px } from './Drawing/Utility'
import { Color } from './Drawing/Color'
import { Rect } from './Drawing/Rect'
import { Text } from './Drawing/Text'

class DrawCanvas {
  context: CanvasRenderingContext2D
  boxs: Rect[] = []
  texts: Text[] = []

  constructor(context: CanvasRenderingContext2D) {
    this.context = context
    this.update = this.update.bind(this)
  }
  setText() {
    const box1 = new Rect(this.context, 10, 10, 50, 50, new Color(200, 0, 0, 1))
    this.boxs.push(box1)
    const text1 = new Text(this.context, 10, 10, 50, 50, new Color(0, 0, 0, 1))
    text1.setText(30, 'あいうえおあいうえおあいうあいうえおあいうえおあいうあいうえおあいうえおあいうあいうえおあいうえおあいう')
    this.texts.push(text1)
  }
  update() {
    requestAnimationFrame(this.update)
    this.context.clearRect(0, 0, mm2px(210), mm2px(297))
    this.boxs.forEach(box => {
      box.draw()
    })
    this.texts.forEach(text => {
      text.draw()
    })
  }
}

const canvas = document.querySelector('.pdf-content') as HTMLCanvasElement
const context = canvas.getContext('2d')!
const dc = new DrawCanvas(context)
dc.setText()
dc.update()
```




