#js

---
2021-09-06

# Swiper

スライドライブラリ、jQuery 依存しないやつらしい。

```shell
$ npm init vite@latest swiper-sample
$ cd swiper-sample
$ npm install swiper
```

今のバージョンはいろいろなフレームワークに対応していて、いろいろ変わってた。

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
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div class="swiper-slide">
          <img src="./images/nanahushi.png" />
        </div>
        <div class="swiper-slide">
          <img src="./images/icon_fukuro.png" />
        </div>
        <div class="swiper-slide">
          <img src="./images/icon_choucho.png" />
        </div>
        <div class="swiper-slide">
          <img src="./images/icon_ahiru.png" />
        </div>
        <div class="swiper-slide">
          <img src="./images/icon_tentoumushi.png" />
        </div>
        <div class="swiper-slide">
          <img src="./images/icon_usagi.png" />
        </div>
        <div class="swiper-slide">
          <img src="./images/icon_yamachu.png" />
        </div>
        <div class="swiper-slide">
          <img src="./images/minomushi.png" />
        </div>
        <div class="swiper-slide">
          <img src="./images/harinezumi.png" />
        </div>
      </div>
    </div>


  </body>
  <script type="module" src="/main.js"></script>
</html>
```

style.css
```css
.swiper-container {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background: url(./images/section4.png);
  width: 100vw;
  height: 100vh;
  overflow-x: hidden;
}

.swiper-wrapper img {
  width: 100px;
  object-fit: contain;
}
```


main.js

```js
import Swiper, { Autoplay } from 'swiper'
import 'swiper/css'

import './style.css'

Swiper.use([Autoplay])

new Swiper('.swiper-container', {
  loop: true,
  direction: 'horizontal',
  slidesPerView: 6,
  speed: 4000,
  autoplay: {
    delay: 1000,
  },
})
```

Autoplay を import して、 Swiper.use([Autoplay]) としないと動かなくなってた。

![[Pasted image 20210906174726.png]]



