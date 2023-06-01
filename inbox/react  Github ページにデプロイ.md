#js/react #github 

---
2021-12-27

# Github ページに React をデプロイする


https://qiita.com/KoheiShingaiHQ/items/b4bf8dd47a99e5d14caf


```shell
$ npm i gh-pages --save-dev

```

package.json

```json
"scripts" : {
   "build": "react-scripts build",
   "deploy": "gh-pages -d build"   // 事前にビルドする必要がある

   "deploy": "npm run build && gh-pages -d build"  // 一緒にやるか
}
```

```shell
$ npm run build
$ npm run deploy
```

これでページに反映される。

deploy コマンドが github にデプロイまでしてくれた。

github ページでは gh-pages ブランチが反映する。setting でブランチを変更できるが、gh-pages でいいんじゃね?

