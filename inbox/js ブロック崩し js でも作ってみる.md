#js

---
2021-11-01

# Breakout Game

```shell
$ npm init vite@latest breakout
$ cd breakout
$ npm i
$ npm run dev
```

あれ? css が効いてる。まいっか。

途中からエラーになった。

main.js  -> main.jsx にしたら動くようになった。

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
    <canvas id="game" width=400 height=500></canvas>
    <script type="module" src="/main.jsx"></script>
  </body>
</html>

```


main.jsx

```jsx
import './style.css'

const canvas = document.querySelector('#commandame')
let ctx
let gseq
let px = 200
const py = 420
const pw = 40
const ph = 20
let time
let mouseX
let bx, by, spdx, spdy, lastx, lasty
const bw = 7
const bh = 7
const blw = 78
const blh = 30
const blf = new Array(25)
let score
let mcount
let bexist

const gameInit = () => {
  if (canvas.getContext) {
    ctx = canvas.getContext('2d')
  }
  gseq = 1
  bx = 100
  by = 250
  spdx = 2
  spdy = 2
  for(let i=0; i<25; i++) blf[i] = 1
  score = 0
  mcount = 0
  bexist = 1
}
const clearDisp = () => ctx.clearRect(0, 0, canvas.clientWidth, canvas.height)

const draw = () => {
  clearDisp()
  if (gseq == 0) {
    gameTitle()
  } else if (gseq == 1) {
    gamePlay()
  } else {
    gameOver()
  }
}

const gameTitle = () => {
  playerMove()
  playerDisp()
  blockDisp()
  ctx.fillStyle = 'rgb(255, 255, 255)'
  ctx.font = '40pt Arial'
  ctx.fillText('BREAKOUT', 50, 300)
  if(mcount % 120 > 40) {
    ctx.fillStyle = 'rgb(255, 255, 20)'
    ctx.font = '20pt Arial'
    ctx.fillText('Click to start', 120, 350)
  }
  mcount++
  for(let i=0; i<25; i++) blf[i] = 1
}

const gamePlay = () => {
  playerMove()
  playerDisp()
  ballMove()
  ballDisp()
  blockDisp()
  scoreDisp()
}

const gameOver = () => {
  ctx.fillStyle = "rgb(255, 255, 255"
  ctx.font = "20pt Arial"
  ctx.fillText(`Score: ${score}`, 40, 200)
  ctx.fillStyle = 'rgb(255, 10, 20)'
  ctx.font = '40pt Arial'
  ctx.fillText('GAME OVER', 40, 250)
  if(mcount % 120 > 40) {
    ctx.fillStyle = 'rgb(255, 255, 20)'
    ctx.font = '20pt Arial'
    ctx.fillText('click me', 150, 350)
  }
  mcount++
}

const playerMove = () => {
  px = mouseX
  if (px + pw > canvas.width) px = canvas.width - pw
}
const playerDisp = () => {
  ctx.fillStyle = 'rgb(255, 255, 255)'
  ctx.fillRect(px, py, pw, ph)
}

const ballMove = () => {
  lastx = bx
  lasty = by
  bx += spdx
  by += spdy
  if (0 > by) {
    spdy *= -1
  }
  if (canvas.height - bh < by) {
    //spdy *= -1
    gseq = 2
  }
  if (0 > bx || canvas.width - bw < bx) {
    spdx *= -1
  }
  if (bx > px && bx < px + pw && by > py && by < py + ph) {
    spdy = -Math.abs(spdy)
    if(bexist == 0) {
      score += 30
      for(let i=0; i<25; i++) blf[i] = 1
    }
  }
}

const ballDisp = () => {
  ctx.fillStyle = 'rgb(255,255,255)'
  ctx.fillRect(bx, by, bw, bh)
}

const blockDisp = () => {
  let xx, yy
  bexist = 0
  for(let i=0; i<25; i++) {
    xx = (i % 5) * (blw+2)
    yy = parseInt(i / 5) * (blh+2) + blh*2

    if(blf[i] == 1) {
      bexist = 1
      blockHitCheck(i, xx, yy)
      ctx.fillStyle = `hsl(${parseInt(i/5) * 40}, 100%, 50%)`
      ctx.fillRect(xx, yy, blw, blh)
    }
  }
}

const blockHitCheck = (i, xx, yy) => {
  if(!(bx > xx && bx < xx+blw && by > yy && by < yy+blh)) return
  blf[i] = 0
  score += 10
  if(lastx > xx && lastx < xx+blw) {
    spdy *= -1
  } else if(lasty > yy && lasty < yy+blh) {
    spdx *= -1
  } else {
    spdx *= -1
    spdy *= -1
  }

}
const scoreDisp = () => {
  ctx.fillStyle = "rbg(255, 255, 255)"
  ctx.font = '20pt Arial'
  ctx.fillText(`Score: ${score}`, 20, 40)
}

gameInit()
time = setInterval(draw, 10)

const getMousePosition = (canvas, event) => {
  const rect = canvas.getBoundingClientRect()
  return {
    x: event.clientX - rect.left,
    y: event.clientY - rect.top,
  }
}

canvas.addEventListener('mousemove', (event) => {
  mouseX = getMousePosition(canvas, event).x
})

canvas.addEventListener('click', (event) => {
  console.log(gseq)
  if(gseq == 0) {
    gameInit()
  }
  if(gseq == 2) {
    gseq = 0
  }
})
```

style.css

```css
#commandame {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: [[2c3e50]];
  margin-top: 10px;
  background-color: black;
}

```

![[Pasted image 20211101163824.png]]