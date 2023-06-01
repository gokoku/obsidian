#css

---
2021-08-24

# スクロールでページが下から被さってくるエフェクト

## css の position: sticky を使う

この sticky はバックテクスチャが固定になるようにして、コンテンツは普通にスクロールするようにするようだ。

てゆーか、コンテンツは通常スクロールで操作。バックグラウンドが固定あるいはパララックスする的な感じのようだ。


## こんなコンテンツ

このようなページが下から上がってくる紙芝居みたいな感じ。

![[Pasted image 20210824135305.png]]

![[Pasted image 20210824135316.png]]

![[Pasted image 20210824135327.png]]

![[Pasted image 20210824135337.png]]



## 仕組み

https://coliss.com/articles/build-websites/operation/css/css-position-sticky-how-it-really-works.html

position: sticky を使う。

top: 0 を一緒に設定すると、その要素が top : 0 にきたらそこでスクロールしなくなる。

そのままスクロールすると次の要素がやってくる。

その要素が top: 0 にくると、スクロールが止まり、次の要素が上がってくる。

次々と下から紙を差し込んで見せる紙芝居のような効果が作れる。


## 実装

```shell
$ npm init vite@latest sticky-sample
$ cd sticy-sample
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
      <div id="scene0">
        <p>scene0</p>
      </div>
      <div id="scene1">
        <p>scene1</p>
      </div>
      <div id="scene2">
        <p>scene2</p>
      </div>
      <div id="scene3">
        <p>scene3</p>
      </div>
      <div id="scene4">
        <p>scene4</p>
      </div>
    </div>

    <script type="module" src="/main.js"></script>
  </body>
</html>
```

### style.css

```css
body {
  padding: 0;
  margin: 0;
}
.container {
  padding: 0;
  margin: 0;
}
[[scene0]] {
  position: sticky;
  top: 0;

  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 1000px;
  font-size: 10rem;
  color: black;
  background-color: ghostwhite;
}
[[scene1]] {
  position: sticky;
  top: 0;

  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 1000px;
  font-size: 10rem;
  color: white;
  background-color:darkgreen;
}
[[scene2]] {
  position: sticky;
  top: 0;

  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 1000px;
  font-size: 10rem;
  color: white;
  background-color: coral;
}
[[scene3]] {
  position: sticky;
  top: 0;

  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 1000px;
  font-size: 10rem;
  color: white;
  background-color: cornflowerblue;
}

[[scene4]] {
  position: sticky;
  top: 0;

  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 1000px;
  font-size: 10rem;
  color: white;
  background-color: crimson;
}
```

### main.js

```js
import './style.css'
```




