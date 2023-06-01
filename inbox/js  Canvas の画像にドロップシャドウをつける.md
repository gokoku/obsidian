---
type: note
---

#js

---
2023-04-26  13:56

# js  Canvas の画像にドロップシャドウをつける

```js
context.filter = 'drop-shadow(5px 5px 20px rgba(0,0,0,0.5))'
context.drawImage( photo, x, y, width, height, ax, ay, width, height)
context.filter = 'none'
```

'none' で解除しないと、このあと全部にドロップシャドウが付いてしまうようだ。

