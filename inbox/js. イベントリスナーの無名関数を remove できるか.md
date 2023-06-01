---
type: note
---

#js

---
2022-09-21  18:08

# js. イベントリスナーの無名関数を remove できるか

https://stackoverflow.com/questions/3106605/removing-an-anonymous-event-listener

みんな知恵出し合ってる。

jQuery 3.0 使うのがいいような気がしてきた。

その中で、こんなのがあった。

```js
myElem.addEventListener("click", myFunc = function() { /*do stuff*/ });

/*things happen*/

myElem.removeEventListener("click", myFunc);
```

どーだ?

```