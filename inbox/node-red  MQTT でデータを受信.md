#node-red

---
2021-12-10

# MQTT でデータ受信

ESP32 でセンサーからデータを MQTT でパブリッシュしているところから。

[[arduino   ESP32 でMQTT 通信]]

Node-RED の network から mqtt in を使って受信する。

ESP32 ではトピックを myTopic にしている。

![[Pasted image 20211210150850.png]]

![[Pasted image 20211210150837.png]]

JSON 文字列で来るので、パースしてやる。

![[Pasted image 20211210151151.png]]

function で温度を抽出する。

因みに json は、

```json
{"devices": "ESP32","envsensor":{"temperature":25.6,"humidity":31.0,"heatindex":25.0},"lampsensor":{"blue":2.3,"yellow":2.3,"red":2.2}}
```

function のコードは、

温度の抽出。

```js
msg.payload = msg.payload.envsensor.temperature
return msg;
```

その他の値。

```js
// 湿度
msg.payload = msg.payload.envsensor.humidity

// フォトセンサー電圧
msg.payload = msg.payload.lampsensor.blue

msg.payload = msg.payload.lampsensor.yellow

msg.payload = msg.payload.lampsensor.red
```

![[Pasted image 20211210151605.png]]