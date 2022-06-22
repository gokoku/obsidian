#css #js/vite

---
2021-07-21

# How to create Google's Material Design Text Input Field using CSS and JavaScript?

https://dev.to/murtuzaalisurti/how-to-create-google-s-material-design-text-input-field-animation-38n


vite.js の vanila.js でコビーしてみた。

```shell
$ npm init vite@latest

vanila
project: material-design-text-input

$ cd material-disign-text-input
$ npm install
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
    <title>Material-design</title>
  </head>
  <body>
    <div class="input-contain">
      <input type="text" id="fname" name="fname" autocomplete="off" value="" aria-labelledby="placeholser-fname">
      <label class="placeholder-text" for="fname" id="placeholser-fname">
        <div class="text">First Name</div>
      </label>
    </div>
    <script type="module" src="/main.js"></script>
  </body>
</html>

```

style.css

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
:root {
  font-size: 62.5%;
}
body {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.input-contain {
  position: relative;
}
input {
  height: 5rem;
  width: 40rem;
  border: 2px solid black;
  border-radius: 1rem;
}
input:focus {
  outline: none;
  border-color: blueviolet;
}

input:focus + .placeholder-text .text,
:not(input[value=""]) + .placeholder-text .text {
  background-color: white;
  font-size: 1.1rem;
  color: black;
  transform: translate(0, -170%);
}

input:focus + .placeholder-text .text {
  border-color: blueviolet;
  color: blueviolet;
}

.placeholder-text{
  position:absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  border: 3px solid transparent;
  background-color: transparent;
  pointer-events: noene;
  display: flex;
  align-items: center;
}

.text {
  font-size: 1.4rem;
  padding: 0 0.5rem;
  background-color: transparent;
  transform: translate(0);
  color: black;
  transition: transform 0.15s ease-out,
              font-size 0.15s ease-out,
              background-color 0.2s ease-out,
              color 0.15s ease-out;
}
input, .placeholder-text {
  font-size: 1.4rem;
  padding: 0 1.2rem;
}
@media (max-width: 40rem) {
  input {
    width: 70vw;
  }
}
```

main.js

```js
import './style.css'

let input_element = document.querySelector('input')

input_element.addEventListener('keyup', () => {
  input_element.setAttribute('value', input_element.value)
})
```

素晴らしい。
良く出来てる。

![[Pasted image 20210721111451.png]]

![[Pasted image 20210721111506.png]]

![[Pasted image 20210721111518.png]]

