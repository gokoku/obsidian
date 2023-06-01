#css/tailwind 


[初めてでもわかるTailwindcss入門(1) | アールエフェクト](https://reffect.co.jp/html/tailwindcss-for-beginners)

## セットアップ

```shell
$ npm install tailwindcss
```

css/style.css ファイルを作る。

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

これをビルドする先のディレクトリをつくる。

```shell
$ mkdir public/css
```

ビルドする。

```shell
npx tailwind build ./css/style.css -o ./public/css/style.css
```

これを scripts にコマンドを書いとく。

```json
"scripts": {
    "build": "tailwind build css/style.css -o public/css/style.css"
}
```

### カスタマイズ用設定ファイルの作成

```shell
$ npx tailwind init
```
