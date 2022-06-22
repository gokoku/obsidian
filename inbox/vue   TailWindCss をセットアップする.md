#js/vue 


[初めてのtailwindcss (Vue.js + PurgeCSS) - Qiita](https://qiita.com/hisako135/items/49c05633de4dadfcd627)

```shell
$ vue create-app my-proj

$ cd my-proj
$ yarn add tailwindcss
```

postcss.config.js を作成してプラグインとして設定する。

```js
const tailwindcss = require('tailwindcss')
const autoprefixer = require('autoprefixer')

module.exports = {
  plugins: [
    tailwindcss,
    autoprefixer,
  ]
}
```

src/assets/tailwind.css に tailwind のスタイル用CSSファイルを作成するとのこと。

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

main.js で tailwind.css を import

```js
import Vue from 'vue'
import App from './App.vue'

import '@/assets/tailwind.css'
```

* * *

## @apply でまとめる

src/assets/tailwind.css で設定する。
utilities の前に読み込ませないといけないらしい。

```css
@tailwind base;
@tailwind components;

.btn-teal {
  @apply bg-teal-500 text-white py-2 px-4 rounded w-full;
}
.btn-teal:hover {
  @apply bg-teal-700 transition-colors transition-250 transition-linear;
}

@tailwind utilities;
```

* * *

## 簡単カスタマイズ

```shell
$ yarn tailwind init

or

$ npx tailwind init
```

tailwind.config.js が出来るのでここでカスタマイズ。
どうやるかは後ほど。

* * *

## 使ってない CSS を削除

```shell
$ yarn add @fullhuman/postcss-purgecss
```

postcss.config.js

```js
const autoprefixer = require('autoprefixer');
const tailwindcss = require('tailwindcss');
const purgecss = require('@fullhuman/postcss-purgecss')({
  // テンプレートファイルへのパス。今回は'./src/**/*.vue'
  content: [
    './src/**/*.vue'
  ],
  // https://medium.com/@kyis/vue-tailwind-purgecss-the-right-way-c70d04461475
  defaultExtractor: (content) => {
    const contentWithoutStyleBlocks = content.replace(/<style[^]+?<\/style>/gi, '')
    return contentWithoutStyleBlocks.match(/[A-Za-z0-9-_/:]*[A-Za-z0-9-_/]+/g) || []
  },
  whitelistPatterns: [ /-(leave|enter|appear)(|-(to|from|active))$/, /^(?!cursor-move).+-move$/, /^router-link(|-exact)-active$/ ],
})

module.exports = {
  plugins: [
    autoprefixer,
    tailwindcss,
    // 開発中はビルド時間がかかってしまうので、productionの時のみ実行
    ...process.env.NODE_ENV === 'production'
    ? [purgecss]
    : []
  ]
}
```

かなりシェイプアップするらしい。
yarn build して dist/css/ を覗く。
