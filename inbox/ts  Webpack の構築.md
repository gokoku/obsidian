#ts #webpack 

---
2021-12-22

# Webpack を使う

```
├── dist
│   └── index.js
├── node_modules
├── package-lock.json
├── package.json
├── ts
├── tsconfig.json
└── webpack.config.js
```

```shell
$ npm init -y

$ npm i --save-dev webpack webpack-cli typescript ts-loader serve
```

index .html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
</head>
<body>
  <script src="/dist/index.js" type="text/javascript"></script>
</body>
</html>
```

webpack.config.js

```js
const { resolve } = require('path')

module.exports = {
  mode: 'development',
  devtool: 'inline-source-map',
  entry: resolve(__dirname, 'ts/index.ts'),
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
        },
      },
    ],
  },
}
```

tsconfig.json

このファイルがあるところが tsc の起点になるようだ。これがないと、別階層までコンパイルしようとする。

```json
{
  "compilerOptions": {
    "sourceMap": true,
    "target": "ES2015",
    "lib": ["DOM", "ESNext"],
    "module": "ES2015",
    "esModuleInterop": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noImplicitReturns": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
  }
}
```

package.json

```json

  "scripts": {
    "build": "webpack",
    "serve": " serve ./ -p 3000"
  },
```

```shell
$ npm run build

$ npm run serve
```

