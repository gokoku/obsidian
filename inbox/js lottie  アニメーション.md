#js/anime 

---
2021-08-31

# アニメーションツール

Afttr Effects でつくるシェイプアニメーションツール


作り方
https://rokujuudo.com/blog/entry/aftereffecs_bodymovin


https://lottiefiles.com/preview


https://techblog.raccoon.ne.jp/archives/1575508778.html

https://photoshopvip.net/121220



## 実装

```shell
$ npm init vite@lastest lottie-sample
$ cd lottie-sample
$ npm install lottie-web
$ npm run dev
```


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
    <div id="lottie" style="width:50vw"></div>
    <script type="module" src="/main.js"></script>
  </body>
</html>
```


```js
import './style.css'
import lottie from 'lottie-web'

animation = lottie.loadAnimation({
  container: document.querySelector('#lottie'),
  renderer: 'svg',
  loop: true,
  autoplay: true,
  path: './loading_pstore.json',
  name: 'Loading',
})
```

loading_pstore.json が lottie file 

後で Affter Effect でつくろうね。


