#ts #webpack 

---
2021-12-21

# Error: error:0308010C:digital envelope routines::unsupported

何じゃこのエラー。

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

package.json

```json
{
  "name": "browser-app",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack",
    "serve": "serve ./ -p 3000"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "serve": "^13.0.2",
    "ts-loader": "^9.2.5",
    "typescript": "^4.3.5",
    "webpack": "^5.50.0",
    "webpack-cli": "^4.7.2"
  }
}
```

```shell
$ npm run build

> browser-app@1.0.0 build
> webpack

/Users/george/Desktop/ts/browser-app/node_modules/loader-runner/lib/LoaderRunner.js:146
                if(isError) throw e;
                            ^

Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:67:19)
    at Object.createHash (node:crypto:130:10)

```

大量のエラーが出る。

https://stackoverflow.com/questions/69692842/error-message-error0308010cdigital-envelope-routinesunsupported


```shell

$ export NODE_OPTIONS=--openssl-legacy-provider

$ npm run build

> browser-app@1.0.0 build
> webpack

asset index.js 3.55 KiB [emitted] (name: main)
./ts/index.ts 101 bytes [built] [code generated]
./ts/sum.ts 127 bytes [built] [code generated]
webpack 5.50.0 compiled successfully in 1118 ms
```

ウソのように成功した。

?

OpenSSL 3.0 のハッシュ関数の問題らしい。


```
 opensslErrorStack: [ 'error:03000086:digital envelope routines::initialization error' ],
  library: 'digital envelope routines',
  reason: 'unsupported',
  code: 'ERR_OSSL_EVP_UNSUPPORTED'
```

https://qiita.com/cnloni/items/1c83cac956599fb24158

webpack(v5) のエラーとのこと。

5.65.0 で治ってた。





