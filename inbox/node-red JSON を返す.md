---
type: note
---

#node-red 

---
2022-07-01  14:59

# node-red JSON を返す

https://cookbook.nodered.jp/http/serve-json-content

![[Pasted image 20220701150021.png]]

```
[{"id":"c8107088.37ef9","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello-json","method":"get","swaggerDoc":"","x":120,"y":620,"wires":[["4e8237da.b17dc8"]]},{"id":"4e8237da.b17dc8","type":"template","z":"3045204d.cfbae","name":"page","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"{ \"Hello\": \"World\" }","x":290,"y":620,"wires":[["65401623.9abfe8"]]},{"id":"65401623.9abfe8","type":"change","z":"3045204d.cfbae","name":"Set Headers","rules":[{"t":"set","p":"headers","pt":"msg","to":"{}","tot":"json"},{"t":"set","p":"headers.content-type","pt":"msg","to":"application/json","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":450,"y":620,"wires":[["f7d3e35a.082c2"]]},{"id":"f7d3e35a.082c2","type":"http response","z":"3045204d.cfbae","name":"","x":610,"y":620,"wires":[]}]
```

template の中

![[Pasted image 20220701151023.png | 400]]

change の中身

![[Pasted image 20220701151103.png | 400]]

http response は何もしない。

![[Pasted image 20220701151126.png | 400]]