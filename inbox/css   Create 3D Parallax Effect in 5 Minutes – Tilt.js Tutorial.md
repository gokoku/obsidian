#css #js

---
2021-07-30

# Create 3D Parallax Effect in 5 Minutes – Tilt.js Tutorial

https://redstapler.co/tilt-js-parallax-tutorial/

カードの角丸が効かないときのチップス。

![tilt.js glare edge](https://redstapler.co/wp-content/uploads/2021/01/tilt-js-tutorial-glare-edge.jpg)

```css
.js-tilt-glare {
   border-radius: 18px;
}
```


![[Pasted image 20210730104203.png]]

## セットアップ

```shell
$ npm init vite@latest
$ cd card-css
$ npm install vanilla-tilt
$ npm run dev
```

##### index.html

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
    <div class="card">
      <div class="card-image"></div>
      <div class="card-text">
        <span class="date">4 days ago</span>
        <h2>Post One</h2>
        <p>Lorem ipsum dolor sit amet consectetur, Ducimus, repudiandae temporibus omnis illum maxime quod deserunt eligendi dolor</p>
      </div>
      <div class="card-stats">
        <div class="stat">
          <div class="value">4<sup>m</sup></div>
          <div class="type">read</div>
        </div>
        <div class="stat border">
          <div class="value">5123</div>
          <div class="type">views</div>
        </div>
        <div class="stat">
          <div class="value">32</div>
          <div class="type">comments</div>
        </div>
      </div>
    </div>

    <script type="text/javascript" src="/node_modules/vanilla-tilt/dist/vanilla-tilt.js"></script>
    <script type="module" src="/main.js"></script>
  </body>
</html>

```

##### main.js

```js
import './style.css'

VanillaTilt.init(document.querySelectorAll('.card'), {
  glare: true,
  reverse: true,
  'max-glare': 0.5,
})
```

##### style.css

```css
body {
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}


.card {
  display: grid;
  grid-template-columns: 300px;
  grid-template-rows: 210px 210px 80px;
  grid-template-areas: "image" "text" "stats";

  border-radius: 18px;
  background: white;
  box-shadow: 5px 5px 15px rgba(0,0,0,0.9);
  font-family: roboto;
  text-align: center;

  transition: 0.5s ease;
  cursor: pointer;
  margin: 30px;
}

.card-image {
  grid-area: image;
  background: url("images/img1.jpg");
  border-top-left-radius: 15px;
  border-top-right-radius: 15px;
  background-size: cover;
}

.card-text {
  grid-area: text;
  margin: 1rem;
}
.card-text .date {
  color: rgb(255, 7, 110);
  font-size: 13px;
}
.card-text p {
  color: grey;
  font-size: 15px;
  font-weight: 300;
}
.card-text h2 {
  margin-top: 0px;
  margin-bottom: 1rem;
  font-size: 28px;
}

.card-stats {
  grid-area: stats;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 1fr;

  border-bottom-left-radius: 15px;
  border-bottom-right-radius: 15px;
  background: rgb(255, 7, 110);
}
.card-stats .stat {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  color: white;
  padding: 10px;
}
.card-stats .border {
  border-left: 1px solid rgb(172, 26, 87);
  border-right: 1px solid rgb(172, 26, 26);
}
.card:hover {
  transform: scale(1.15);
  box-shadow: 5px 5px 15px rgba(0.0.0.0.6);
}
```

