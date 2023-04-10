---
type: note
---

#js

---
2022-11-24  10:09

# js cropper.js の使い方

https://fengyuanchen.github.io/cropperjs/

https://cly7796.net/blog/javascript/try-using-cropper-js/
<div class="rich-link-card-container"><a class="rich-link-card" href="https://cly7796.net/blog/javascript/try-using-cropper-js/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://cly7796.net/blog/wp-content/uploads/2019/10/cut.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Cropper.jsを使ってみる</h1>
		<p class="rich-link-card-description">
		ページ上で画像のトリミングなどができるJavaScriptライブラリ「Cropper.js」を使ってみます。 設定方法 Githubのページから一式
		</p>
		<p class="rich-link-href">
		https://cly7796.net/blog/javascript/try-using-cropper-js/
		</p>
	</div>
</a></div>

```shell
$ npm init vite@lastest
$ cd cropperjs-sample
$ npm i
$ code .
$ npm run dev
```

cropperjs のインストール

```shell
$ npm i cropperjs
```

index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
  </head>
  <body>
    <div class="main">
      <div class="result">
        <p>切り抜き後の画像</p>
        <img id="result-img">
      </div>

      <div id="cropper-area">
        <img id="cropper-img" src="./chagpon.png">
      </div>

      <button id="crop-btn">切り抜き</button>
    </div>

    <script type="module" src="/main.js"></script>
  </body>
</html>
```


style.css
```css
.main {
  display: flex;
  align-items: center;
}
.result {
  width: 45%;
  height: 100%;
  text-align: center;
}
#cropper-area {
  width: 45%;
  height: 100%;
}
.cropper-area img {
  display: block;
  max-width: 100%;
}
button {
  width: 10%;
  height: 100px;
}
```


main.js

```js
import './style.css'
import './node_modules/cropperjs/dist/cropper.css'
import './node_modules/cropperjs/dist/cropper'

const cropperImg = document.getElementById('cropper-img')
const cropper = new Cropper(cropperImg, {
  viewMode: 1,
  aspectRatio: 4/3,
  zoomable: false
})

document.getElementById('crop-btn').addEventListener('click', () => {
  const resultImgUrl = cropper.getCroppedCanvas().toDataURL()
  const result = document.getElementById('result-img')
  result.src = resultImgUrl
})
```

たったこれだけで、

![[Pasted image 20221124111707.png]]

# 切り抜きの永続化

- 画像データそのものの保存
	resultImgUrl のなかは src= で入れられる、base64 データ。
	これを丸々 DB に入れても永続化は可能だ。
- 値の保存
	- x, y, width, height が取れれば、Canvas の image から切り抜きを再現できるのではないだろうか
	- まず、cropper js で値が取れるか調べる。
		- 取得するメソッドがある!!
```js
  const data = cropper.getData()
  console.log("x:", data.x, " y: ", data.y, " width: ", data.width, " height: ", data.height, " rotate: ", data.rotate)
```

値の保存で切り取りが永続化出来ることがわかった。

## canvas で元画像からの切り抜き

```html
<canvas id="canvas" width="400" height="300"></canvas>
```

```js
const draw = (sx, sy, sWidth, sHeight) => {
  const canvas = document.querySelector("#canvas")
  const ctx = canvas.getContext("2d")

  const img = new Image()
  img.src = "./chagpon.png"
  img.onload = () => {
    ctx.drawImage(img, sx, sy, sWidth, sHeight, 0, 0, 400, 300)
  }
}
```

sx, sy, sWidth, sHeight に cropper.js の detData() で取得した値を入れると、切り抜き画像が再現できる。

index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
  </head>
  <body>
    <div class="main">
      <div class="result">
        <p>切り抜き後の画像を canvas に表示</p>
        <canvas id="canvas" width="400" height="300"></canvas>
      </div>

      <div id="cropper-area">
        <img id="cropper-img" src="./chagpon.png">
      </div>

      <button id="crop-btn">切り抜き</button>
    </div>

    <script type="module" src="/main.js"></script>
  </body>
</html>
```

style.css
```css
.main {
  display: flex;
  align-items: center;
}
.result {
  width: 45%;
  height: 100%;
  text-align: center;
}
#cropper-area {
  width: 45%;
  height: 100%;
}
.cropper-area img {
  display: block;
  max-width: 100%;
}
button {
  width: 10%;
  height: 100px;
}
```


main.js
```js
import './style.css'
import './node_modules/cropperjs/dist/cropper.css'
import './node_modules/cropperjs/dist/cropper'

const cropperImg = document.getElementById('cropper-img')
const cropper = new Cropper(cropperImg, {
  viewMode: 1,
  aspectRatio: 4/3,
  zoomable: false
})

document.getElementById('crop-btn').addEventListener('click', () => {
  const data = cropper.getData()
  console.log("x:", data.x, " y: ", data.y, " width: ", data.width, " height: ", data.height, " rotate: ", data.rotate)
  draw(data.x, data.y, data.width, data.height)

})

const draw = (sx, sy, sWidth, sHeight) => {
  const canvas = document.querySelector("#canvas")
  const ctx = canvas.getContext("2d")

  const img = new Image()
  img.src = "./chagpon.png"
  img.onload = () => {
    ctx.drawImage(img, sx, sy, sWidth, sHeight, 0, 0, 400, 300)
  }
}
```

![[Pasted image 20221124120010.png]]





