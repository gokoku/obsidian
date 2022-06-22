#ts #webpack 

---
2022-03-14  08:38

# ts  複数ファイルの import を解決するには webpack

本番で import 解決させるとネストが深い時はラウンドタイムが長くなってあかんらしい。
なので、一つにした方が色々と都合がいい。

```shell
$ npm -g i typescript

$ npm init -y
$ npm install --save-dev webpack webpack-cli typescript ts-loader serve

$ npm tsc -init
```

```
.
├── dist
│   ├── index.html
│   └── index.js
├── node_modules
├── package-lock.json
├── package.json
├── src
│   ├── index.ts
│   └── sum.ts
├── tsconfig.json
└── webpack.config.js
```

webpack.config.js を用意する。

```js:webpack.config.js
const { resolve } = require('path')

module.exports = {
  mode: 'development',
  devtool: 'inline-source-map',
  entry:resolve(__dirname, 'src/index.ts'),
  output: {
    filename: 'index.js',
    path: resolve(__dirname, 'dist'),
  },
  resolve: {
    extensions: ['.ts', '.js'],
  },
  module: {
    rules: [
      {
        test: /\.ts$/,
        use: {
          loader: 'ts-loader',
        }
      }
    ]
  }
}
```
tsconfig.json
```json:tsconfig.json
	"sourceMap": true
```

package.json
```json:package.json
  "scripts": {
    "build": "webpack",
    "dev": "webpack --watch",
    "serve": "serve ./dist -p 3000"
  },
```

dist/index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <script src="./index.js"></script>
</body>
</html>
```

これで、ts の import が出来る。

```ts:src/index.ts
import { sum } from './sum'

console.log(sum(1,2))
```

```ts:src/sum.ts
export const sum = (a: number, b: number): number => a + b
```

開発

```shell
$ npm run dev

-----
$ npm run serve
```

