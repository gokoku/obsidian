#raspberry_pi  #node-red 



[https://cookbook.nodered.jp/http/handle-url-parameters](https://cookbook.nodered.jp/http/handle-url-parameters)

単一のエンドポイントでパラメータ的なハンドリングしたい。

```bash
http://people.local:1880/recommends/goods/0
http://people.local:1880/recommends/goods/1
http://people.local:1880/recommends/goods/2
http://people.local:1880/recommends/goods/3 ...
```

```bash
get /recommends/goods/:num
```

アクセスは、msg.req.params

---

![](image-kndsxnoy.png)

/hello-params/:name で GET 受信。

function の中は

```bash
msg.payload = msg.req.params

return msg;
```

出力は

![[Pasted image 20210525174603.png]]

:param の名前で値が受信出来る。

