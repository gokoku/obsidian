#ruby/rails

---
2021-06-25

# Notice を一定時間後にスーッと消す
このためだけに jQuery は入れたくない。

https://knowledge-blog.net/articles/29

この人のコードを参考にした。

\#e にメッセージが入るとして。

どこに置くかどーしよか、こーしてみるか。

app/javascript/packs/application.js

```js
// notice を一定時間たったら消す
import 'fadeOut.js'
```

app/javascript/fadeOut.js

```js
document.addEventListener('DOMContentLoaded', () => {
  notice = document.querySelector('#e')
  if (notice) {
    notice.style.opacity = 1
    setTimeout(() => {
      id = setInterval(() => {
        const opacity = notice.style.opacity
        if (opacity > 0) {
          notice.style.opacity = opacity - 0.01
        } else {
          notice.style.display = 'none'
          clearInterval(id)
        }
      }, 20)
    }, 4000)
  }
})
```

