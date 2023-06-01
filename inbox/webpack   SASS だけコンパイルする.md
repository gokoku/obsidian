#webpack #js #sass #css

---
2021-08-26

# Webpack で Sass コンパイル

今回の案件で特殊なことは、css ファイルを .css.liquid にするところかな。

https://www.webdesignleaves.com/pr/css/sass_webpack.html

```shell
$ npm install autoprefixer css-loader fibers mini-css-extract-plugin --save-dev
$ npm install postcss postcss-loader sass sass-loader --save-dev
$ npm install webpack webpack-cli
```

webpack.config.js

```js
const PATH = require('path')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')

module.exports = {
  entry: './sass.js',
  output: {
    path: PATH.resolve(__dirname, 'assets'),
    filename: '_tmp.js',
  },
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: [
          MiniCssExtractPlugin.loader,
          {
            loader: 'css-loader',
            options: {
              url: false,
            },
          },
          {
            loader: 'postcss-loader',
            options: {
              postcssOptions: {
                plugins: [['autoprefixer', { grid: true }]],
              },
            },
          },
          {
            loader: 'sass-loader',
            options: {
              implementation: require('sass'),
              sassOptions: {
                fiber: require('fibers'),
              },
            },
          },
        ],
      },
    ],
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: 'application.css.liquid',
      ignoreOrder: true,
    }),
  ],
  devtool: 'source-map',
  watchOptions: {
    ignored: /node_modules/,
  },
}
```


* rules は下から処理されるとのこと、sass-loader --> postcss-loader --> css-loader と変換される
* sass-loader で fiber をセットすると並列処理になって速くなるとのこと。
* postcss-loader ではベンダープレフィックスを出力してくれるために autoprefixer を使った。
* postcss-loader で grid を true にすると grid のベンダープレフィックスが入る
* css-loader では 画像のリンクエラーになったので url: false にして止まらないようにした。
* css のファイル名を特別な名前にするので、MiniCssExtractPlugin を使った。
* watch 対象から node_modules を外した。


### autoprefixer のブラウザオプションは package.json に記述

バージョンアップでこうなったとのこと。

package.json

```json
  "browserslist": [
    "last 2 versions",
    "ie >= 11"
  ]
```

### npm scripts

mode はオプションで指定するのは良さげだ。

package.json

```json
  "scripts": {
    "build": "webpack --mode production",
    "dev": "webpack --mode development",
    "watch": "webpack --mode development --watch",
    "sass-ver": "sass --version"
  },
```

### package.json

```json
{
  "devDependencies": {
    "autoprefixer": "^10.3.2",
    "css-loader": "^6.2.0",
    "fibers": "^5.0.0",
    "mini-css-extract-plugin": "^2.2.0",
    "postcss": "^8.3.6",
    "postcss-loader": "^6.1.1",
    "sass": "^1.38.1",
    "sass-loader": "^12.1.0",
    "webpack": "^5.51.1",
    "webpack-cli": "^4.8.0"
  },
  "scripts": {
    "build": "webpack --mode production",
    "dev": "webpack --mode development",
    "watch": "webpack --mode development --watch",
    "sass-ver": "sass --version"
  },
  "browserslist": [
    "last 2 versions",
    "ie >= 11"
  ]
}
```






