#js/vite

---
2021-07-26

# Viteで始めるモダンで高速な開発環境構築


https://ics.media/entry/210708/
![jQueryからTypeScript・Reactまで - Viteで始めるモダンで高速な開発環境構築](https://ics.media/entry/210708/images/eyecatch.png)

ヴィート

ES modulesはInternet Explorer 11（以下IE11）以外

しかし、ビルドした結果はIE11でも動かすことができます。


```shell
$ npm init vite@latest
```

```shell
$ cd project
$ npm install
$ npm run dev
```

## Viteのみ（バニラJS=フレームワークなし）のサンプル

すごいなーこの人。イラスト描くんだよ。Blender やってたよ。

![[Pasted image 20210726114449.png]]


![[Pasted image 20210726110118.png]]

###### index.html

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
  </head>
  <body>
    <div id="app">
      <div id="sample">
        <div id="todoInput">
          <input type="text" id="itemInput" placeholder="新しいtodo" />
          <button id="addButton">追加</button>
        </div>
        <ul id="todoList">
        </ul>
      </div>
    </div>
    <script type="module" src="/main.js"></script>
  </body>
</html>

```

###### main.js

```js
import $ from 'jquery'
import './style.css'
import { addTodo } from './TodoList'

const newInput = $('#itemInput]]')
const addButton = $('#addButton]]')

addButton.on('click', () => {
  const text = newInput.val()
  if (!text) return
  addTodo(text)
  newInput.val('')
})

addTodo('買い物')
addTodo('部屋の掃除')
addTodo('Vite入門')
```

###### TodoList.js

```js
import { TodoItem } from './TodoItem'
import $ from 'jquery'

const LIST_UL_ID = '#todoList]]'
const items = []

const createTodoItemLi = (item) => {
  return $(`
    <li id="${item.id}">
      <p>${item.text}</p>
      <button>削除</button>
    </li>
  `)
}

export const addTodo = (text) => {
  const item = new TodoItem(text)
  items.push(item)
  const ul = $(LIST_UL_ID)
  const li = createTodoItemLi(item)
  li.find('button').on('click', () => {
    removeTodo(item.id)
  })
  ul.append(li)
  return item.id
}

export const removeTodo = (id) => {
  const index = items.findIndex((item) => item.id === id)
  if (index !== -1) {
    items.splice(index, 1)
  }
  const li = $('#' + id)
  li.remove()
}
```

###### TodoItem.js

```js
let lastInstanceId = 0

export class TodoItem {
  constructor(text) {
    this.id = 'todo' + lastInstanceId
    this.text = text
    lastInstanceId++
  }
}

```

###### style.css

```css
body {
  margin: 0;
  margin: 0;
  padding: 30px 10%;
}

* {
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: [[2c3e50]];
}

h1 {
  font-size: 24px;
  text-align: left;
}

.info {
  font-size: 13px;
  padding: 10px 0 40px 0;
  text-align: left;
}

#sample {
  border: 2px solid [[2c3e50]];
  border-radius: 4px;
  max-width: 600px;
  margin: auto;
}

#todoInput]] {
  display: flex;
  padding: 8px;
  justify-content: space-around;
  background-color: [[2c3e50]];
}

#todoInput]] input {
  width: calc(100% - 120px);
  height: 28px;
  padding: 4px;
  font-size: 16px;
  border: none;
  border-radius: 2px;
}
#todoInput]] button {
  width: 100px;
  height: 29px;
  border: 2px solid #fff;
  border-radius: 3px;
  background-color: transparent;
  color: #fff;
  font-weight: bold;
}

#todoList]] {
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 2px;
}

#todoList]] li {
  display: flex;
  align-items: center;
  padding: 0 12px;
}

#todoList]] li p {
  width: calc(100% - 100px);
  font-size: 18px;
  text-align: left;
}

#todoList]] li button {
  width: 100px;
  height: 28px;
  border: none;
  background-color: #e26172;
  color: #fff;
  font-weight: bold;
  border-radius: 4px;
}
```






## Vite + Sass(SCSS)

```shell
$ npm init vite@latest

$ cd project
$ npm install -D sass
```

後は、style.css --> style.scss 

main.js を import './style.scss'　にするだけで、ホットリロードでコンパイルしてくれる。

```css
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: [[2c3e50]];
  margin-top: 60px;

  h1 {
    color: olivedrab;
  }
}
```

超簡単 SASS 開発環境

## Vite + TypeScript

```shell
$ npm init vite@latest

? Project name:  hello-vite-ts
? Select a framework

> vanilla

? Select a variant

> vanilla-ts
```

Three.js のサンプル。

```shell
$ npm install three @types/three

$ npm install -D sass
```

![[Pasted image 20210726113425.png]]

###### src/main.ts

```ts
import { createThreeScene } from './createThreeScene'
import './style.scss'

const app = document.querySelector<HTMLDivElement>('#app')!
const threeCanvas = createThreeScene(app.clientWidth, app.clientHeight)
app.appendChild(threeCanvas)

```
###### src/createThreeScene.ts

```ts
import * as THREE from 'three'

export const createThreeScene = (width = 800, height = 600) => {
  const renderer = new THREE.WebGLRenderer()
  renderer.setSize(width, height)
  const scene = new THREE.Scene()

  const camera = new THREE.PerspectiveCamera(45, width/height, 1, 10000)
  camera.position.set(0,0,1000)

  const geometry = new THREE.BoxGeometry(250, 250, 250)
  const material = new THREE.MeshStandardMaterial({color: 0xff0000})
  const box = new THREE.Mesh(geometry, material)
  box.position.z = -5
  scene.add(box)

  const light = new THREE.DirectionalLight(0xffffff)
  light.position.set(1,1,1)
  scene.add(light)

 const tick = (): void => {
   requestAnimationFrame(tick)

   box.rotation.x += 0.01
   box.rotation.y += 0.01

   renderer.render(scene, camera)
 }
 tick()
 
 return renderer.domElement
}
```

###### src/style.scss

```css
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #afb5bb;
  margin: 0;
  height: 100%;
}

```

