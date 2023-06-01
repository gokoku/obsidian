#js 

---
2021-11-23

# 「クリエイティブコーディングの教科書」


<div class="rich-link-card-container"><a class="rich-link-card" href="https://zenn.dev/baroqueengine/books/a19140f2d9fc1a/viewer" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://res.cloudinary.com/zenn/image/upload/s--u2YyvEl5--/g_center%2Ch_280%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYm9va19jb3Zlci83N2QxZDUwOWQ4LmpwZw==%2Cw_200/v1627283836/default/og-base-book_yz4z02.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">準備｜クリエイティブコーディングの教科書</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://zenn.dev/baroqueengine/books/a19140f2d9fc1a/viewer
		</p>
	</div>
</a></div>




## 01 準備

```shell
$ npm init latest@latest preparation    <-- npx だと error なのだが??
$ cd preparation

$ npm i p5

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
    <div id="app"></div>
    <script type="module" src="/main.js"></script>
  </body>
</html>
```

style.css

```css
html, body {
  margin: 0;
  padding: 0;
}
body {
  overflow: hidden;
  background: [[292a33]];
}

```

main.js

```js
import './style.css'
import p5 from 'p5'

const sketch = (p) => {
  p.setup = () => {
    p.createCanvas(p.windowWidth, p.windowHeight)
    p.circle(p.width / 2, p.height / 2, 50)
  }
  p.draw = () => {}
}

new p5(sketch, 'app')
```

これで行ける!!

![[Pasted image 20211123171128.png]]

## アニメーション

```js
import './style.css'
import p5 from 'p5'

const sketch = (p) => {
  let r, x, s

  p.setup = () => {
    p.createCanvas(p.windowWidth, p.windowHeight)

    r = p.min(p.width, p.height) / 6
    x = r
    s = '0'
  }
  p.mouseClicked = () => {}
  p.draw = () => {
    x += 10
    if (x > p.width + r) {
      x = -r
    }
    p.clear()
    p.circle(x, p.height / 2, r * 2)

    if (p.frameCount % 30 === 0) {
      s = p.frameRate().toFixed(0)
    }

    p.push()
    p.noStroke()
    p.fill(200)
    p.text(s, 20, 20)
    p.pop()
  }
}

new p5(sketch, 'app')
```


## 円の描画

https://zenn.dev/baroqueengine/books/a19140f2d9fc1a/viewer/1acd2a

