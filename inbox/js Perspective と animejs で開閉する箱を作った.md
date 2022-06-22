#js #js/anime #css 

---
2021-08-05

# Perspective と animejs で開閉する箱を作った

https://github.com/gokoku/animejs-open-dox

```shell
$ npm init vite@latest
$ cd animejs-open-door
$ npm i animejs -S
$ npm run dev
```

![[Pasted image 20210805170319.png]]

css で perspective 3D の設定をしておく。

anime で transition を指定すると、いい感じに動いてくれる。

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
    <div id="app">
      <div class="box">
        <div class="door door-l" >左</div>
        <div class="door door-r" >右</div>
        <div class="door door-t" >上</div>
      </div>
    </div>
    <script type="module" src="/main.js"></script>
  </body>
</html>
```

#### style.css

* transform-style で preserve-3d を設定する。
* perspective で奥行きを設定する。大きいとパースの掛かりが小さくなる。
* transorm-origin    どこを軸にして動かすかを指定する。
* rotate で回転させる。これを animejs で設定したりして動きを制御する





```css
body {
  background-color: #333;
}
#app {
  color: #eee;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
}

.box {
  transform-style: preserve-3d;
  perspective: 1000px;
  width: 250px;
  height: 200px;
  background-color: cadetblue;
  font-size: 3rem;
  font-weight: bold;
  cursor: pointer;
}
.door {
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgba(230, 200, 0, 0.7);
  position: absolute;
}
.door-l {
  transform-origin: 0% 50%;
  top: 0;
  left: 0;
  width: 50%;
  height: 100%;
}
.door-r {
  transform-origin: 100% 50%;
  top: 0;
  right: 0;
  width: 50%;
  height: 100%;
}
.door-t {
  transform-origin: 50% 0%;
  top: 0;
  right: 0;
  width: 100%;
  height: 50%;
}

```

#### main.js

```js
import './style.css'
import anime from 'animejs/lib/anime.js'

const box = document.querySelector('.box')
const door_l = document.querySelector('.door.door-l')
const door_r = document.querySelector('.door.door-r')
const door_t = document.querySelector('.door.door-t')
let flag = false

box.onclick = (e) => {
  flag = !flag
  if (flag) {
    open()
  } else {
    close()
  }
}

const open = () => {
  anime({
    targets: [door_l],
    rotateY: -120,
    duration: 4000,
    elasticity: 800,
    loop: false,
    direction: 'normal',
  })
  anime({
    targets: [door_r],
    rotateY: 120,
    duration: 4000,
    elasticity: 800,
    loop: false,
    direction: 'normal',
  })
  anime({
    targets: [door_t],
    rotateX: 120,
    duration: 4000,
    elasticity: 800,
    loop: false,
    direction: 'normal',
  })
}

const close = () => {
  anime({
    targets: [door_l, door_r, door_t],
    rotateY: 0,
    rotateX: 0,
    duration: 2000,
    loop: false,
    easing: 'linear',
  })
}
```