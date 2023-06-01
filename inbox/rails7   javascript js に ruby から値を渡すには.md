---
type: note
---

#ruby/rails #js #ts

---
2022-06-01  09:32

# rails7   javascript js に ruby から値を渡すには

data Property を介して js サイドに値を渡す。

```html
<div id="dat" data-json="<%= current_user.info.to_json %>"></div>
```


```js
const dat = JSON.parse(document.getElementById('dat').dataset.json)
```

これで行こう。
- dataset. とは
	- https://developer.mozilla.org/ja/docs/Web/API/HTMLElement/dataset
	- HTML data-* はプロパティと同じ扱いが出来る。
	- DOM では dataset.* で取得できる。

