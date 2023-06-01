#js 


## Luxon

https://moment.github.io/luxon/

momentjs でローカルが難しかったらしい。その他色々改善したかったからつくったとのこと。

https://qiita.com/bobu_web/items/2cecc3fdb7d2b0942ef1

## Spacetime


[https://laravel-news.com/spacetime-js](https://laravel-news.com/spacetime-js)

```bash
$ npm i spacetime
```

多機能すぎ

```bash
$ node
> const spacetime = require('spacetime')
> spacetime.now().format('nice')
'Nov 9th, 10:02am'
```



公式でも、Luxon のようなモダンなやつを使ってちょとのこと
![](image-kmmquhkk.png)

https://momentjs.com/docs/[[/-project-status/recommendations]]

## 使ってみる

```js
moment().add(-9, 'hours').format('YYYY-MM-DDTHH:mm:ssZ')

luxon.dateTime.local().toISO()
```

ところが、garoon では jQuery とぶつかってだめだった。多分