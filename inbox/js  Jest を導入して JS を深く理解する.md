#js

---
2021-09-04

# Jest を導入して JS を深く理解する

jest を使う。CommonJS に対応してないので import を使うなら色々入れるらしい。

https://sbfl.net/blog/2019/01/20/javascript-unittest/

めんどくさいので、require でいく。

Add.js
```js
funciton add(a, b) {
  return a + b
}

module.exports = add
```

sample.test.js
```js
const add = require('./Add')

const add = require('./Add')

describe('add', () => {
  test('add 1 + 2', () => {
    expect(add(1, 2)).toBe(3)
  })
})

```

package.json
```json

"scripts" : {
  "test": "jest"
}
```

実行
```shell
$ npm test

$ npm test sample.test.js
```

## 試すとともに結果をテストで残すようにしようか




