#js 

---
2021-08-02

# キーの名前とプロパティの変数名が同じなら省略できる

知らなかった。ES2015 からは省略出来るとのこと。

```js
const id = 123
const name = "taro"

const json = {
  id: id,
  name: name,
}
```

はこのように省略出来る。

```js
const json = {
  id,
  name,
}
```

??? と慌てないように。