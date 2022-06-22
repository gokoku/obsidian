#js #js/rx 

---
2021-07-21

# How to implement drag & drop using RxJS

https://www.thisdot.co/blog/how-to-implement-drag-and-drop-using-rxjs

https://stackblitz.com/edit/rxjs-drag-and-drop-events

vite.js で vanila.js を立ててみる。

```shell
$ npm init vite@latest

✔ Project name: … rxjs-dd
✔ Select a framework: › vanilla
✔ Select a variant: › vanilla

$ cd rxjs-dd
$ npm install
$ npm install rxjs
$ npm run dev
```


# code

## main.js

```js
import './style.css'

import { fromEvent, combineLatest } from 'rxjs'
import { switchMap, takeUntil, map, tap, last } from 'rxjs/operators'

const appDiv = document.querySelector('#p')
appDiv.innerHTML = `
  <h1>RxJS Drag and Drop</h1>
  <div class="draggable">0</div>
  <div class="draggable">1</div>
  <div class="draggable">2</div>
`
const mouseMove$ = fromEvent(document, 'mousemove')
const mouseUp$ = fromEvent(document, 'mouseup')

const draggableElements = document.querySelectorAll('.draggable')

Array.from(draggableElements).forEach(createDraggableElement)

function createDraggableElement(element) {
  const mouseDown$ = fromEvent(element, 'mousedown')

  const dragStart$ = mouseDown$
  const dragMove$ = dragStart$.pipe(
    switchMap((start) =>
      mouseMove$.pipe(
        map((moveEvent) => ({
          originalEvent: moveEvent,
          deltaX: moveEvent.pageX - start.pageX,
          deltaY: moveEvent.pageY - start.pageY,
          startOffsetX: start.offsetX,
          startOffsetY: start.offsetY,
        })),
        takeUntil(mouseUp$),
      ),
    ),
  )

  const dragEnd$ = dragStart$.pipe(
    switchMap((start) =>
      mouseMove$.pipe(
        map((moveEvent) => ({
          originalEvent: moveEvent,
          deltaX: moveEvent.pageX - start.pageX,
          deltaY: moveEvent.pageY - start.pageY,
          startOffsetX: start.offsetX,
          startOffsetY: start.offsetY,
        })),
        takeUntil(mouseUp$),
        last(),
      ),
    ),
  )

  combineLatest([
    dragStart$.pipe(
      tap((event) => {
        element.dispatchEvent(new CustomEvent('mydragstart', { detail: event }))
      }),
    ),
    dragMove$.pipe(
      tap((event) => {
        element.dispatchEvent(new CustomEvent('mydragmove', { detail: event }))
      }),
    ),
    dragEnd$.pipe(
      tap((event) => {
        element.dispatchEvent(new CustomEvent('mydragend', { detail: event }))
      }),
    ),
  ]).subscribe()

  dragMove$.subscribe((move) => {
    const offsetX = move.originalEvent.x - move.startOffsetX
    const offsetY = move.originalEvent.y - move.startOffsetY
    element.style.left = offsetX + 'px'
    element.style.top = offsetY + 'px'
  })
}

Array.from(draggableElements).forEach((element, i) => {
  element.addEventListener('mydragstart', () =>
    console.log(`mydragstart on element #${i}`),
  )
  element.addEventListener('mydragmove', (event) =>
    console.log(
      `mydragmove on element #${i}`,
      `delta: (${event.detail.deltaX}, ${event.detail.deltaY})`,
    ),
  )

  element.addEventListener('mydragend', (event) =>
    console.log(
      `mydragend on element #${i}`,
      `delta: (${event.detail.deltaX}, ${event.detail.deltaY})`,
    ),
  )
})

```


## style.css

```css
h1,h2 {
  font-family Lato;
}

.draggable {
  position: absolute;
  cursor: move;
  width: 32px;
  height:32px;
  left:90px;
  color: white;
  background-color: orange;
  display:flex;
  justify-content: center;
  align-items: center;
}

.draggable:nth-child(2) {
  background-color: blueviolet;
}
.draggable:nth-child(3) {
  background-color: yellowgreen;
}
```

![[Pasted image 20210721143321.png]]