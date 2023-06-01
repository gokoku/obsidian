#ts

---
2022-02-15  18:19

# ts   Drawing tool 作り初めプロトタイプ

https://kernhanda.github.io/tutorial-typescript-canvas-drawing/

```shell
$ mkdir dwaing01 
$ cd drawing01 
$ npm i -D typescript 
$ mkdir src 
$ touch src/index.ts
$ mkdir dist
$ touch dist/index.html
```

```
.
├── node_modules
├── dist
├── package-lock.json
├── package.json
└── src
    └── index.ts
```

tsconfig.json

```json:tsconfig.json
{
  "compilerOptions": {
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "target": "es5",
    "sourceMap": true,
    "lib": [
      "es5",
      "dom"
    ],
    "noUnusedLocals": true,
    "module": "commonjs"
  }
}
```

package.json
```json:package.json
"scripts": {
    "build": "tsc",
    "dev": "tsc -w"
  },
```

dist/index.html
```html:dist/index.html
<html>

<head>
    <title>Canvas demo</title>
</head>
<link rel="stylesheet" href="style.css">
<body>
    <div class="container">
        <div class="image-container">
            <canvas id="canvas"
                width="600"
                height="600"
                style="border:1px solid #d3d3d3;">
            </canvas>
            <p id="clear">clear</p>
        </div>
        <div class="text-container"></div>
    </div>
    <script src="main.js"></script>
</body>

</html>
```

dist/style.css
```css:dist/style.css
* {
  margin: 0;
  padding: 0;
}
body {
  font-family: Verdana, "ヒラギノ角ゴ ProN W3", "Hiragino Kaku Gothic ProN", "メイリオ", Meiryo, sans-serif;
}
.container {
  display: flex;
  width: 100%;
  max-width: 1200px;
  margin: 10px auto;
}
.image-container {
 width: 620px;
}
.image-container p#clear {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 50px;
  background-color: #ccc;
}

.text-container {
  width: 170px;
  height: 600px;
  color: #fff;
  padding: 10px;
  overflow: auto;
  background-color: darkslategrey;
}
```


src/main.ts
```ts:src/main.ts
interface Tarray {
  x: number
  y: number
  dragging: boolean
}

class DrawingApp {
  private canvas: HTMLCanvasElement
  private context: CanvasRenderingContext2D
  private dataContainer: HTMLDivElement
  private paint: boolean
  private data: Tarray[] = []

  private clickX: number[] = []
  private clickY: number[] = []
  private clickDrag: boolean[] = []

  constructor() {
    let canvas = document.getElementById('canvas') as HTMLCanvasElement
    let context = canvas.getContext('2d') as CanvasRenderingContext2D
    let dataContainer = document.querySelector('.text-container') as HTMLDivElement
    context.lineCap = 'round'
    context.lineJoin = 'round'
    context.strokeStyle = '#aa5500'
    context.lineWidth = 3

    this.canvas = canvas
    this.context = context
    this.dataContainer = dataContainer

    this.paint = false
    this.redraw()
    this.createUserEvents()
  }

  private createUserEvents() {
    let canvas = this.canvas

    canvas.addEventListener('mousedown', this.pressEventHandler)
    canvas.addEventListener('mousemove', this.dragEventHandler)
    canvas.addEventListener('mouseup', this.releaseEventHandler)
    canvas.addEventListener('mouseout', this.releaseEventHandler)

    canvas.addEventListener('touchstart', this.pressEventHandler)
    canvas.addEventListener('touchmove', this.dragEventHandler)
    canvas.addEventListener('touchend', this.releaseEventHandler)
    canvas.addEventListener('touchcancel', this.releaseEventHandler)

    const button = document.getElementById('clear')
    if (button) {
      button.addEventListener('click', this.clearEventHandler)
    }
  }

  private redraw() {
    let clickX = this.clickX
    let context = this.context
    let clickDrag = this.clickDrag
    let clickY = this.clickY
    for (let i = 0; i < clickX.length; i++) {
      context.beginPath()
        if (clickDrag[i] && i) {
        context.moveTo(clickX[i - 1], clickY[i - 1])
      } else {
        context.moveTo(clickX[i] - 1, clickY[i])
      }
      context.lineTo(clickX[i], clickY[i])
      context.stroke()
    }
    context.closePath()
  }

  private addClick(x: number, y: number, dragging: boolean) {
    this.clickX.push(x)
    this.clickY.push(y)
    this.clickDrag.push(dragging)

    const text: Tarray = {x, y, dragging}
    this.displayText(text)
  }

  private clearCanvas() {
    this.context.clearRect(0, 0, this.canvas.width, this.canvas.height)
    this.clickX = []
    this.clickY = []
    this.clickDrag = []
  }
  private clearText() {
    this.dataContainer.innerHTML = ''
    this.data = []
  }
  private clearEventHandler = () => {
    this.clearCanvas()
    this.clearText()
  }

  private displayText(text: Tarray) {
    this.data.push(text)
    let tmp = ''
    this.data.forEach((element) => {
      tmp += `x:${element.x}  y:${element.y}  ${element.dragging} <br>`
    });
    if(this.data.length > 30) {
      this.data.shift()
    }
    this.dataContainer.innerHTML = tmp
  }

  private releaseEventHandler = () => {
    this.paint = false
    this.redraw()
  }

/*   private cancelEventHandler = () => {
    this.paint = false
  }
 */
  private pressEventHandler = (e: MouseEvent | TouchEvent) => {
    let mouseX = (e as TouchEvent).changedTouches ?
                  (e as TouchEvent).changedTouches[0].pageX :
                  (e as MouseEvent).pageX
    let mouseY = (e as TouchEvent).changedTouches ?
                  (e as TouchEvent).changedTouches[0].pageY :
                  (e as MouseEvent).pageY
    mouseX -= this.canvas.offsetLeft
    mouseY -= this.canvas.offsetTop

    this.paint = true
    this.addClick(mouseX, mouseY, false)
    this.redraw()
  }

  private dragEventHandler = (e: MouseEvent | TouchEvent) => {
    let mouseX = (e as TouchEvent).changedTouches ?
                  (e as TouchEvent).changedTouches[0].pageX :
                  (e as MouseEvent).pageX
    let mouseY = (e as TouchEvent).changedTouches ?
                  (e as TouchEvent).changedTouches[0].pageY :
                  (e as MouseEvent).pageY
    mouseX -= this.canvas.offsetLeft
    mouseY -= this.canvas.offsetTop

    if (this.paint) {
      this.addClick(mouseX, mouseY, true)
      this.redraw()
    }
    e.preventDefault()
  }
}

new DrawingApp()
```

![[Pasted image 20220215183403.png]]

