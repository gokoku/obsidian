#ts 

---
2022-04-08  18:27

# ts　ts  draw library 開発中 角丸Rectを描くクラス作った


<div class="rich-link-card-container"><a class="rich-link-card" href="http://blog.chatlune.jp/2018/02/06/canvas-rounded-rectangle/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('http://blog.chatlune.jp/wp-content/uploads/2018/02/rect.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">HTML5 Canvasで角丸四角を描いて、グラーデーションで塗り潰してみた</h1>
		<p class="rich-link-card-description">
		Canvasで角丸四角を描画する手順を紹介いたします。引数を与えれば色々な大きさと形の描画ができる関数にしてあります。ただ単色で描くのも味気ないで、グラデーションにも対応させております。サンプルで動かしてみてよかったら使ってみてください。
		</p>
		<p class="rich-link-href">
		http://blog.chatlune.jp/2018/02/06/canvas-rounded-rectangle/
		</p>
	</div>
</a></div>

このお方のおかげで角丸描けるようになった。

```ts:RectRound.ts
import { Property, DrawObject } from './DrawObject'

export class RectRound extends DrawObject {
  radius: number = 20
  constructor(context: CanvasRenderingContext2D, property: Property) {
    super(context, property)
    this.radius = property.option.radius
  }

  draw() {
    this.context.beginPath()
    this.context.lineWidth = 1
    this.context.strokeStyle = this.color
    this.context.fillStyle = this.color
    this.context.moveTo(this.x, this.y + this.radius)
    this.context.arc(this.x + this.radius,
                    this.y + this.height - this.radius,
                    this.radius,
                    Math.PI,
                    Math.PI * 0.5, true)
    this.context.arc(this.x + this.width - this.radius,
                    this.y + this.height - this.radius,
                    this.radius,
                    Math.PI * 0.5,
                    0, true)
    this.context.arc(this.x + this.width - this.radius,
                    this.y + this.radius,
                    this.radius,
                    0,
                    Math.PI * 1.5, true)
    this.context.arc(this.x + this.radius,
                    this.y + this.radius,
                    this.radius,
                    Math.PI * 1.5,
                    Math.PI, true)
    this.context.closePath()
    this.context.stroke()
    this.context.fill()
  }

  dragable(x: number, y: number): boolean {
    return true
  }

  drag(x: number, y: number) {
  }
}
```

![[Pasted image 20220408182854.png]]

