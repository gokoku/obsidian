#js

[https://www.codegrid.net/articles/2017-jest-1/](https://www.codegrid.net/articles/2017-jest-1/)

```shell
$ mkdir jest_sample
$ cd jest_sample
$ yarn init -y
$ yarn add --dev jest
```

package.json
```json
{
    "scripts": "jest"
```

./sum.js
```js
module.exports = function (a, b) {
  return a + b
}
```

./sum.test.js
```js
const sum = require('./sum')

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3)
})
```

```shell
$ yarn test
```