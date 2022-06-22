---
type: note
---

#ts #js/vite 

---
2022-06-01  22:06

# ts Simple-drawing tutorial

```shell
$ npm init vite@latest

simple-drawing
vanilla ts

$ cd simple-drawing
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
    <section class="container">
      <div id="toolbar">
        <h1>Draw.</h1>
        <label for="stroke">Stroke</label>
        <input id="stroke" name="stroke" type="color">
        <label for="lineWidth">Line Width</label>
        <input id="lineWidth" name="lineWidth" type="number" value="5">
        <button id="clear">Clear</button>
      </div>
      <div class="drawing-board">
        <canvas id="drawing-board"></canvas>
      </div>
    </section>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>

```


`src / style.css`

```css
body {
  margin: 0;
  padding: 0;
  height: 100%;
  overflow: hidden;
  color: white;
}

h1 {
  background: #7f7fd5;
  background: -webkit-linear-gradient(to right, #91eae4, #86a8e7, #7f7fd5);
  background: linear-gradient(to right, #91eae4, #86a8e7, #7f7fd5);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.container {
  height: 100vh;
  display: flex;
}

#toolbar {
  display: flex;
  flex-direction: column;
  padding: 10px;
  width: 70px;
  background-color: #202020;
}

#toolbar * {
  margin-bottom: 6px;
}

#toolbar label {
  font-size: 12px;
}

#toolbar input {
  width: 90%;
}

#toolbar button {
  background-color: #1565c0;
  border: none;
  border-radius: 4px;
  color: white;
  padding: 2px;
}

```

`src / main.ts`

```ts
import './style.css'

const canvas = document.getElementById('drawing-board') as HTMLCanvasElement
const toolbar = document.getElementById('toolbar') as HTMLElement
const ctx = canvas.getContext('2d')

const canvasOffsetX = canvas.offsetLeft
const canvasOffsetY = canvas.offsetTop

canvas.width = window.innerWidth - canvasOffsetX
canvas.height = window.innerHeight - canvasOffsetY

console.log(`canvas offsetX: ${canvasOffsetX}`)
console.log(`canvas offsetY: ${canvasOffsetY}`)
console.log(`canvas width: ${canvas.width}`)
console.log(`canvas height: ${canvas.height}`)

let isPainting = false
let lineWidth = 5
let startX
let startY

toolbar.addEventListener('click', (e: MouseEvent) => {
  const target = e.target as HTMLElement
  if(!target) return
  if(target.id === 'clear') {
    ctx?.clearRect(0, 0, canvas.width, canvas.height)
    console.log("clear")
  }
})

toolbar.addEventListener('change', (e: Event) => {
  const target = e.target as HTMLInputElement
  if(!target) return
  if(target.id === 'stroke') {
    ctx!.strokeStyle = target.value
    console.log(`strokeStyle: ${target.value}`)
  }
  if(target.id === 'lineWidth') {
    lineWidth = Number(target.value)
    console.log(`lineWidth: ${target.value}`)

  }
})

const draw = (e: MouseEvent) => {
  if(!isPainting) return
  ctx!.lineWidth = lineWidth
  ctx!.lineCap = 'round'

  ctx!.lineTo(e.clientX - canvasOffsetX, e.clientY - canvasOffsetY)
  ctx!.stroke()

}


canvas.addEventListener('mousedown', (e: MouseEvent) => {
  console.log(`mousedown: ${e.clientX} ${e.clientY}`)
  isPainting = true
  startX = e.clientX
  startY = e.clientY
})

canvas.addEventListener('mouseup', (e: MouseEvent) => {
  console.log("mouseup")
  isPainting = false
  ctx?.stroke()
  ctx?.beginPath()
})

canvas.addEventListener('mousemove', draw)
```


![[Pasted image 20220601220911.png]]