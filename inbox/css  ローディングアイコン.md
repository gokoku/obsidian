#css 

---
2021-11-01

#  ローディングアイコン

![[Pasted image 20211101095224.png]]

```shell
$ npm init vite@latest loding-icon
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
    <i class="loading-icon"></i>

    <script type="module" src="/main.js"></script>
  </body>
</html>

```

main.js

import css がブラウザで失敗するようになっていた。
このように修正する。

```js
import style from './style.css' assert { type: 'css' }
document.adoptedStyleSheets = [style]
```

ここはよくわからん。他では失敗する。jsx にすると、何もしなくてもいいようだ。

style.css

```css
.loading-icon {
  box-sizing: border-box;
  width: 15px;
  height: 15px;
  border-radius: 50%;
  box-shadow:
    0 -30px 0 #e,     /*  上  */
    21px -21px 0 #d,  /* 右上 */
    30px 0 0 #c,      /*  右  */
    21px 21px 0 #b,   /* 右下 */
    0 30px 0 #a,      /*  下  */
    -21px 21px 0 #999,  /* 左下 */
    -30px 0 0 #666,     /*  左  */
    -21px -21px 0 #000; /* 左上 */
  animation: rotate 1s steps(8) 0s infinite;
}

@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/****** Base style. ******/
body {
  display: flex;
  height: 100vh;
  justify-content: center;
  align-items: center;
  margin: 0;
}
```

