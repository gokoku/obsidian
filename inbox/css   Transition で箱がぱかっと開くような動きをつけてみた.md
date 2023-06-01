#css

---
2021-08-03

# Transition で箱がぱかっと開くような動きをつけてみた

https://www.sejuku.net/blog/51548

ここ参考になった。どう使えばいいのかに焦点があって機能の説明が展開していくわかりやすさ。

![[Pasted image 20210803115145.png]]

```shell
$ npm init vite@lastest
$ cd picture-story
$ npm install -D sass
$ npm run dev
```

### index.html

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
      <div class="img-u"></div>
      <div class="img-t"></div>
      <div class="img-l"></div>
      <div class="img-r"></div>
      <div class="img-b"></div>
    </div>
    <script type="module" src="/main.js"></script>
  </body>
</html>
```

### main.js

```js
import './style.scss'
```

### style.scss

```scss
@mixin img {
  position: absolute;
  left:40%;
  top: 200px;
  width: 300px;
  height: 300px;
}

.container {
  text-align: center;
  width: 100vw;
  height: 100vh;
  background-color: antiquewhite;
}
.img-u {
  @include img;
  background: url("./images/self.png") no-repeat;
  background-size: cover;
}
.img-l {
  @include img;
  background: url("./images/origin.png") no-repeat;
  background-size: cover;;
  transform-origin: 0% 50%;     /* 左から0% 上から50% 離れた点を基準にする */
  transform: rotateY(140deg);
  transition: 2s;
}
.img-r {
  @include img;
  background: url("./images/origin.png") no-repeat;
  background-size: cover;;
  transition: 2s;
  transform-origin: 100% 50%;     /* 左から100% 上から50% 離れた点を基準にする */
  transform: rotateY(-140deg);
}
.img-t {
  @include img;
  background: url("./images/origin.png") no-repeat;
  background-size: cover;;
  transform-origin: 50% 0%;     /* 左から50% 上から0% 離れた点を基準にする */
  transform: rotateX(140deg);
  transition: 2s;
}
.img-b {
  @include img;
  background: url("./images/origin.png") no-repeat;
  background-size: cover;;
  transition: 2s;
  transform-origin: 50% 100%;     /* 左から100% 上から100% 離れた点を基準にする */
  transform: rotateX(-140deg);
}
```


@keyframes を設定して animation させるのは、もう js のライブラリにしたほうが行くね？ って感じだったので、そうしよう。

