---
type: note
---

#ts

---
2022-06-01  18:08

# ts. Ball をdrag & drop する

```shell
$ npm init vite@latest
y
ball
vanilla
vanilla-ts

$ cd ball
$ npm install
$ npm run dev
```

index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
  </head>
  <body>
    <canvas id="myCanvas" width="500" height="500"></canvas>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>

```


`src/style.css`

```css
* { padding: 0; margin: 0; }

canvas {
  background: #eee;
  display: block;
  margin: 0 auto;
}
```


`src/main.ts`

```ts
import './style.css'

const document = window.document
const canvas = document.querySelector<HTMLCanvasElement>('#myCanvas')!
const ctx = canvas.getContext('2d')!
const ballRadius = 50
let x = 250
let y = 250

const drawBall = () => {
  ctx.beginPath()
  ctx.arc(x, y, ballRadius, 0, Math.PI * 2)
  ctx.fillStyle = '#0095DD'
  ctx.fill()
  ctx.closePath()
}

const draw = () => {
  ctx.clearRect(0, 0, canvas.width, canvas.height)
  drawBall()
}

let move = (e: MouseEvent) => {
  if(! e.target) return
  const rect = (e.target as HTMLCanvasElement).getBoundingClientRect()
  x = e.clientX - rect.left
  y = e.clientY - rect.top
  draw()
}

draw()

canvas.addEventListener('mousedown', () => {
  canvas.addEventListener('mousemove', move)
})

document.addEventListener('mouseup', () => {
  canvas.removeEventListener('mousemove', move)
})

```
![[Pasted image 20220601184130.png]]

