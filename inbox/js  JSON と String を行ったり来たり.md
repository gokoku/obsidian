#js

---
2022-04-12  13:26

# js  JSON と String を行ったり来たり

```js
a = [{"a":1,"b":2,"c":3},{"d":4,"e":5}]
b = JSON.stringify(a)
c = JSON.parse(b)

# c --> [ { a: 1, b: 2, c: 3 }, { d: 4, e: 5 } ]
```
オブジェクトを永続化するのに string にして、
オブジェクトに戻すのに parse する。

