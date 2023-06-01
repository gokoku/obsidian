---
type: note
---

#ts #js

---
2022-06-14  17:39

# ts  iconify を使ってみる
サンプル見本

https://icon-sets.iconify.design/jam/code-sample/



vite で使ってみたい。
```shell
$ npm i -D @iconify/iconify
```

main.ts

```ts
import '@iconify/iconify'

icon.innerHTML = `
<ul class="tool">
  <li><span class="iconify" data-icon="material-symbols:arrow-outward"></span></li>
  <li><span class="iconify" data-icon="material-symbols:square-rounded"></span></li>
  <li><span class="iconify" data-icon="material-symbols:circle-outline"></span></li>
  <li><span class="iconify" data-icon="material-symbols:delete-rounded"></span></li>
</ul>
`

```


style.css

```css
ul.tool li {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ccc;
}
ul.tool li .iconify {
  font-size: 2em;
}
```

![[Pasted image 20220614174236.png]]

## js で innerHTML しなくていいようだ

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
    <div id="tools">
      <ul class="tool">
        <li><span id="edit" class="iconify" data-icon="material-symbols:arrow-outward"></span></li>
        <li><span id="create" class="iconify" data-icon="material-symbols:square-rounded"></span></li>
        <li><span id="delete" class="iconify" data-icon="material-symbols:delete-rounded"></span></li>
      </ul>
    </div>

    <canvas id="myCanvas" width="500" height="500"></canvas>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>
```

main.ts で import '@iconify/iconify' してるだけ。

