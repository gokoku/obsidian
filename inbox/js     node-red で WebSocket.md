#js #node-red

node-red で受信側を用意してみた。

![](image-kn8exyge.png)

![](image-kn8ey804.png)

node で送信してみる。

ws ライブラリをダウンロードする必要があるようだ。

```bash
$ mkdir web_socket
$ cd web_socket
$ npm init -y
$ npm i ws
```

```bash
$ node
> const WebSocket = require('ws')
> const connection = new WebSocket('ws://people.local:1880/ws/example')
```

ここで接続に表示が変わった。


![](image-kn8eyzue.png)

```bash
> connection.send("send data!!")
```

![](image-kn8ezkrx.png)

いきました。

ws プロトコルでネットワーク名とポート同じで開くのか。

あとは http と同じで、どのURLで待つかを設定しただけだ。

切断する。

```bash
> connection.close()
```

![](image-kn8ezxmb.png)
