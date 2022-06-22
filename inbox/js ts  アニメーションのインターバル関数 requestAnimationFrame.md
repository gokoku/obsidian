#js #ts

---
2022-04-06  18:33

# js ts  アニメーションのインターバル関数

setInterval を使うところで、もっと滑らかなものがある。

requestAnimationFrame


```js
let x: number
let y: number
const anim = (context: CanvasRendringContext2D) => {
	requestAnimationFrame(anim)

	context.clearRect(0, 0, window.width, window.height)
	context.fillRect(x, y, 10, 10)
	x += 1
	y += 1
}

```