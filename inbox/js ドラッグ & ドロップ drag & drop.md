---
type: note
---

#js

---
2022-08-01  17:21

# js ドラッグ & ドロップ drag & drop

https://www.digitalocean.com/community/tutorials/js-drag-and-drop-vanilla-js-ja

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="example-parent">
    <div class="example-origin">
      <div
        id="draggable-1"
        class="example-draggable"
        draggable="true"
        ondragstart="onDragStart(event)"
      >
        draggable
      </div>
    </div>
    <div
      class="example-dropzone"
      ondragover="onDragOver(event)"
      ondrop="onDrop(event)"
    >
      dropzone
    </div>
  </div>
  <script src="script.js"></script>
</body>
</html>
```

style.css

```css
.example-parent {
  border: 2px solid #DFA612;
  color: black;
  display: flex;
  font-family: sans-serif;
  font-weight: bold;
}

.example-origin {
  flex-basis: 100%;
  flex-grow: 1;
  padding: 10px;
}

.example-draggable {
  background-color: #4AAE9B;
  font-weight: normal;
  margin-bottom: 10px;
  margin-top: 10px;
  padding: 10px;
}

.example-dropzone {
  background-color: #6DB65B;
  flex-basis: 100%;
  flex-grow: 1;
  padding: 10px;
}

.drag {
  background-color: #DFA612;
}

```

script.js

DragOver は必要だった。
DragOver あっての Drop だった。

```js
const onDragStart = (e) => {
  e.dataTransfer.setData('text/plain', e.target.id);
  e.currentTarget.classList.add('drag');
}

const onDragOver = (e) => {
  e.preventDefault();
}

const onDrop = (e) => {
  const id = e.dataTransfer.getData('text/plain');

  const draggableElement = document.getElementById(id);
  const dropzone = e.currentTarget;
  dropzone.appendChild(draggableElement);
  e.dataTransfer.clearData();
}
```

たったこれだけで、ドラッグ&ドロップで要素を移動させられる。

![[Pasted image 20220801172709.png]]

![[Pasted image 20220801172723.png]]



中の情報だけを取得してみる。

```js
const onDragStart = (e) => {
  e.dataTransfer.setData('text/plain', e.target.id);
  e.currentTarget.classList.add('drag');
}

const onDragOver = (e) => {
  e.preventDefault();
}

const onDrop = (e) => {
  const id = e.dataTransfer.getData('text/plain');
  const draggableElement = document.getElementById(id);
  const element = document.createElement("div")
  element.innerHTML = draggableElement.innerHTML;
  const dropzone = e.currentTarget;
  dropzone.appendChild(element);
  e.dataTransfer.clearData();
}
```

![[Pasted image 20220801172837.png]]


# Todo list

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="example-parent">
    <h1>To-do list</h1>
    <div class="example-origin">
      To-do
      <div
        id="draggable-1"
        class="example-draggable"
        draggable="true"
        ondragstart="onDragStart(event)"
        >
        thing 1
      </div>
      <div
        id="draggable-2"
        class="example-draggable"
        draggable="true"
        ondragstart="onDragStart(event)"
        >
        thing 2
      </div>
      <div
        id="draggable-3"
        class="example-draggable"
        draggable="true"
        ondragstart="onDragStart(event)"
        >
        thing 3
      </div>
      <div
        id="draggable-4"
        class="example-draggable"
        draggable="true"
        ondragstart="onDragStart(event)"
        >
        thing 4
      </div>
    </div>
    <div
      class="example-dropzone"
      ondragover="onDragOver(event)"
      ondrop="onDrop(event)"
      >
      Done
    </div>
  </div>
</body>
<script src="script.js"></script>
</html>
```

```css
一緒
```

script.js

```js
const onDragStart = (e) => {
  e.dataTransfer.setData('text/plain', e.target.id);

  e.currentTarget.classList.add('drag');
}

const onDragOver = (e) => {
  e.preventDefault();
}

const onDrop = (e) => {
  const id = e.dataTransfer.getData('text/plain');
  const draggableElement = document.getElementById(id);
  const dropzone = e.currentTarget;
  dropzone.appendChild(draggableElement);
  e.dataTransfer.clearData();
}
```

![[Pasted image 20220801174458.png]]

戻すにはどうすれば良いのだろうか。

どちらの領域にも ondragover と ondrop を設定すればいい。

```html
  <div class="example-parent">
    <h1>To-do list</h1>
    <div class="example-origin"
      ondragover="onDragOver(event)"
      ondrop="onDrop(event)"
      >
      To-do
      
      <div id="draggable-1" ...
      <div id="draggable-2"...
      <div id="draggable-3"...
      <div id="draggable-4"...
    </div>

    <div
      class="example-dropzone"
      ondragover="onDragOver(event)"
      ondrop="onDrop(event)"
      >
      Done
    </div>
  </div>


```

![[Pasted image 20220801175104.png]]


# HTML ドラッグ & ドロップ API

https://developer.mozilla.org/ja/docs/Web/API/HTML_Drag_and_Drop_API

| イベント  | On イベントハンドラ |
| --------- | ------------------- |
| drag      | ondrag              |
| dragend   | ondragend           |
| dragenter | ondragenter         |
| dragexit  | ondragexit(en-US)   |
| dragleave | ondragleave         |
| dragover  | ondragover          |
| dragstart | ondragstart         |
| drop      | ondrop              |


### interface
- DragEvent
- DataTransfer     ドラッグイベントの状態、ドラッグデータ
- DataTransferItem
- DataTransferItemList


### ドラッグ可能なものを特定する
### ドラッグするデータの定義
### ドラッグ画像の定義
### ドラッグ効果の定義
### ドロップゾーンの定義
### ドロップ効果を扱う
### ドラッグの終了




