---
type: note
---

#js

---
2022-09-26  14:59

# js ランダム値の最大値と最小値の設定

```js

Math.floor( Math.random() * ( 最大値 - 最小値 ) + 最小値 )

```

最大値はその数にならないので、注意。

5 ~ 10 なら 
Math.floor (Math.random() * ( 11 - 5) + 5)

