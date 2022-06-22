#js/vite 

---
2021-07-19

https://vitejs.dev/

# Vite.js



軽量で速い Vue.js ビルドツールらしい。
と思ったら、 webpack の軽量版的なものらしい。

snowpack とどっちがいいか?みたいな感じか?



https://garbanzo.xyz/vite%E5%85%A5%E9%96%80/

vue3 にしか対応してないらしい。

```shell
$ npm init vite-app vite-sample
$ cd vite-sample
$ npm install

$ npm run dev
```

localhost:3000

![[Pasted image 20210719142756.png]]


なんとシンプル!!

![[Pasted image 20210719142834.png]]


```js
$ npm run build
```

dist に作られる。

![[Pasted image 20210719142955.png]]

# 基本的使い方
https://ics.media/entry/210708/

Vue.js だけではないことになっている。

```shell
$ npm init vite@latest
```
![[Pasted image 20210719143442.png]]

```shell
$ cd vite-sample
$ npm install
$ npm run dev
```

## sass

```shell
$ npm install -D sass
```

style.css を style.scss にする。たったこれだけ。

```js
import './style.scss'
```

