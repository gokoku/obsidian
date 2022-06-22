#raspberry_pi  #node-red 


日付取得のライブラリを使いたい。

[https://asuki-yt.hatenablog.jp/entry/node-red-nodegen-nodejs-require](https://asuki-yt.hatenablog.jp/entry/node-red-nodegen-nodejs-require)

moment.js を使う。

./.node-red/settings.js の追記。

```bash
functionGlobalContext: {
        //...
        //...
        //...
        moment:require('moment')
    },
```

```bash
$ cd ~/.node-red
$ npm install moment --save

$ node-red-restart
```

node-red で使ってみる。

![](image-kndt3u0t.png)

```bash
const moment = global.get('moment');

msg.payload = moment().format('dddd');

return msg;
```

![](image-kndt44qq.png)

使えた!! 最高か!!

## そのうち終わるとのこと

Recommendation の中の Luxon を使おう。

settings.js に設定追加する。

```shell:.node-red/settings.js
functionGlobalContext: {
        //...
        //...
        //...
        moment:require('moment'),
				luxon:require('luxon')
    },
```

```shell
$ cd ~/.node-red
$ npm install luxon --save
$ node-red-restart
```

