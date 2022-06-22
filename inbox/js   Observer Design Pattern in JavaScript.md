#js

---
2021-08-10

# Observer Design Pattern in JavaScript

https://betterprogramming.pub/observer-design-pattern-in-javascript-c839ee49add4

```shell
$ npm init vite@latest observer-pattern
vanilla
vanilla

$ cd obser-pattern
$ npm install
$ npm run dev
```

#### index.html

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
    <div class="container">
      <div class="circle"></div>
      <div class="mouse-position"></div>
      <h4>Mouse Position</h4>
      <div class="position"></div>
      <div class="xAxis"></div>
      <div class="yAxis"></div>
      <div class="circle2"></div>
    </div>
    <script type="module" src="/main.js"></script>
  </body>
</html>
```


#### main.js

```js
import './style.css'

class MousePositionObservable {
  constructor() {
    this.subscriptions = []
    window.addEventListener('mousemove', this.handleMouseMove)
  }
  handleMouseMove = (e) => {
    this.subscriptions.forEach((sub) => sub(e.clientX, e.clientY))
  }
  subscribe(callback) {
    this.subscriptions.push(callback)

    return () => {
      this.subscriptions = this.subscriptions.filter((cb) => cb === callback)
    }
  }
}

const mousePositionObservable = new MousePositionObservable()

mousePositionObservable.subscribe((x, y) => {
  const circle = document.querySelector('.circle')
  window.setTimeout(() => {
    circle.style.transform = `translate(${x - 12}px, ${y - 12}px)`
  }, 1000)
})

mousePositionObservable.subscribe((x, y) => {
  const board = document.querySelector('.mouse-position, .position')
  board.innerHTML = `
    <div>
      <div>ClientX: ${x}</div>
      <div>ClientY: ${y}</div>
    </div>
  `
})

mousePositionObservable.subscribe((x, y) => {
  const xAxis = document.querySelector('.xAxis')
  xAxis.style.top = `${y}px`
})

mousePositionObservable.subscribe((x, y) => {
  const yAxis = document.querySelector('.yAxis')
  yAxis.style.left = `${x}px`
})

mousePositionObservable.subscribe((x, y) => {
  const circle2 = document.querySelector('.circle2')
  circle2.style.left = `${x - 40}px`
  circle2.style.top = `${y - 40}px`
})
```


#### style.css

```css
body {
  padding: 0;
  margin: 0;
}
.container {
  position: relative;
  width: 100vw;
  height: 100vh;
  background-color: [[f3df49]];
}
h4 {
  margin: 0;
  padding: 10px;
}
.circle {
  position: absolute;
  background-color: green;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  z-index: 2;
}
.mouse-position {
  position: fixed;
  top: 20px;
  right: 20px;
  width: 200px;
  height: 100px;
  background-color: black;
  border-radius: 4px;
  padding: 4px 16px;
  color: white;
}
.mouse-position h4 {
  color: white;
  margin: 10px 0;
}
.xAxis {
  position: absolute;
  border: 1px dashed orange;
  top: 0;
  width: 100vw;
}
.yAxis {
  position: absolute;
  border: 1px dashed orange;
  height: 100vh;
  top: 0;
}
.circle2 {
  position: absolute;
  border: 1px dashed brown;
  width: 80px;
  height: 80px;
  border-radius: 50%;
}
```

![[Pasted image 20210810142615.png]]


MousePositionObservable に subcribe を追加すると、一斉に配信されて、それぞれが動く!!!

## 記事の中身
Observer パターンはサブスクリプションモデルに従います。

- 購読者 ( Observer 観測者 )が

- イベントかまたはパブリッシャーによって処理されるアクション ( subject )の購読を予約し、

- そして、イベントかアクションが発生すると通知とれます。


subject はイベントかアクションの発生を、全ての観測者たちに向けて一斉配信します。

observer が subject による変更の通知を望まなくなったら、subject から購読解除し、
subject はそれを購読者リストから削除します。



---
Observable が購読者の受け取ったときのアクションを登録して、イベントがあったらそれらのアクションに引数を渡してドライブさせてる。