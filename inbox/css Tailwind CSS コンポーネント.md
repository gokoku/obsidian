#css/tailwind 

## マイコンポーネント


これを使いまわそう。
ここに追加していこう。

src/assets/tailwind.css

```css
@tailwind base;
@tailwind components;

.btn {
  @apply inline-block px-5 py-2 rounded-lg font-semibold text-sm tracking-wider;
}
.btn:focus {
  @apply outline-none shadow-outline;
}
.btn-red {
  @apply bg-red-500 text-white;
}
.btn-red:hover {
  @apply transition duration-300 ease-in-out bg-red-600;
}
.btn-red:active {
  @apply bg-red-700;
}
.btn-teal {
  @apply bg-teal-400 text-white;
}
.btn-teal:hover {
  @apply transition duration-300 ease-in-out bg-teal-500;
}
.btn-teal:active {
  @apply bg-teal-600;
}

.input-text {
  @apply shadow-lg appearance-none border rounded-lg py-2 px-3 text-gray-700;
}

.col-list {
  @apply flex flex-col list-none;
}
.list-item {
  @apply flex items-center bg-white px-2 py-2 mb-2 rounded-lg shadow;
}

@tailwind utilities;
@tailwind screens;
```

* * *

## Vue

```shell
$ yarn add tailwindcss
```

main.js

```js
import '@/assets/tailwind.css'
```

postcss.config.js

```js
const tailwindcss = require('tailwindcss')
const autoprefixer = require('autoprefixer')

module.exports = {
  plugins: [tailwindcss, autoprefixer],
}
```
