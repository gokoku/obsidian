---
type: note
---

#js

---
2022-07-19  15:52

# js 全角文字チェック

```js
if (str.match(/^[^\x01-\x7E\uFF61-\uFF9F]+$/)) {    
  //全角文字
  console.log("全角文字です");
} else {
  //全角文字以外
  console.log("全角文字ではありません");
}

/[^\x00-\x80]/ でいいかな

```


半角の文字数
```js
str.replace(/[^\x00-\x80]/g).length
```


