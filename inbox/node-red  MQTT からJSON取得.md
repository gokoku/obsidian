#node-red #raspberry_pi 

---
2021-12-07

# MQTT から JSON取得して値を抽出

ESP32 から JSON を取得すると、文字列で取得された。

```json
{"devices": "ESP32",
 "envsensor":{"temperature":26.6,"humidity":37.0,"heatindex":26.5},
 "lampsensor":{"blue":2.4,"yellow":2.4,"red":2.3}
}

```

node-red で温度を抽出する。


![[Pasted image 20211207170912.png]]

mqtt in   --> json --> function

function の js

```js
msg.payload = msg.payload.envsensor.temperature

return msg;
```

![[Pasted image 20211207171037.png]]
