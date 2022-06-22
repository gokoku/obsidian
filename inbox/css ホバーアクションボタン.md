#css 


# ボタンのホバーアクションチップ

<https://codepen.io/gokoku/pen/NWxgpMR>

before で塗りつぶしてあって、これをスケール0にして、
ホバーのときにトランジションでスケール1 にして展開させる、知恵ものの記事。

[Button Hover Effects](https://codepen.io/davidjsealey/details/LYGywXv)

```css

:root {
  --primary: [[25abd9]];
}
* {
  box-size: border-box;
}
body {
  background-color: [[dcf5ff]];
  margin: 0;
  padding: 0;
}
.container {
  align-items: center;
  display: flex;
  height: 100vh;
  justify-content: center;
}
.card {
  align-items: center;
  background-color: #f;
  border-radius: 20px;
  box-shadow: 3px 3px 2px 2px #c;
  display: flex;
  flex-wrap: wrap;
  height: 200px;
  justify-content: space-between;
  padding: 50px;
  width: 400px;
}
.button {
  background: none;
  border: solid 2px var(--primary);
  border-radius: 10px;
  box-shadow: 0 5px 15px -5px #b;
  color: var(--primary);
  cursor: pointer;
  font-family: roboto;
  font-size: 14px;
  height: 50px;
  overflow: hidden;
  padding: 5px;
  position: relative;
  transform: perspective(1px);  /* Important to include perspective - gives the button */
  transition: color 0.2s ease-out;
  width: 150px;
}
.button::before {
  content: "";
  position: absolute;
  z-index: -1;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: var(--primary);
  transition: transform 0.3s ease-out;
}

/* Button Specific Styles */
.button--one::before {
  transform-origin: 0 0;
  transform: scaleX(0);
}
.button--two::before {
  transform-origin: 0 0;
  transform: scaleY(0);
}
.button--three::before {
  transform: scaleX(0);
}
.button--four::before {
  transform: scaleY(0);
}

/* Hover Styles */
.button:hover {
  color: #f;
}

.button--one:hover::before {
  transform: scaleX(1);
}
.button--two:hover::before {
  transform: scaleY(1);
}
.button--three:hover::before {
  transform: scaleX(1);
}
.button--four:hover::before {
  transform: scaleY(1);
}
```

```html
<body>
  <div class="container">
    <div class="card">
      <button class="button button--one">Left to Right</button>
      <button class="button button--two">Top to Bottom</button>
      <button class="button button--three">Centre Horizontal</button>
      <button class="button button--four">Centre Vertical</button>
  </div>
  </div>
</body>
```
