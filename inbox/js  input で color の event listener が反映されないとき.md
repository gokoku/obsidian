---
type: note
---

#js

---
2022-10-07  17:38

# js  input で color の event listener が反映されないとき

https://stackoverflow.com/questions/53789296/color-input-box-input-type-color-does-not-work-with-input-event

input でカラーの時、event listener を設定してるのに、どうも反映されない。

こうしたらうまく、反映された。
click イベントも取ってやるのね。

```shell
$ npm init vite@latest

ProjectName : color-event

$ cd color-event
$ npm i
$ npm run dev
```


```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
  </head>
  <body>
    <input type="color" value="#ff0000" id="colorWell">
    <p>Text to be colored</p>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>

```

```js
import './style.css'

const colorWell = document.getElementById("colorWell")
colorWell?.addEventListener('click', updateFirst, false)
colorWell?.addEventListener('input', updateFirst, false)

function updateFirst(e) {
  const p = document.querySelector("p")
  if(p) {
    p.style.color = e.target.value
  }
}
```


![[Pasted image 20221007174149.png]]

![[Pasted image 20221007174207.png]]

