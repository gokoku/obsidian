#js

---
2021-08-02

# import * as name って何?


https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/import


```js
import * as myModule from '/modules/my-module.js'
```

とすると、全ての export を名前空間で使えるようになる。

```js
myModule.getItem()
myModule.setItem('abc')
```

