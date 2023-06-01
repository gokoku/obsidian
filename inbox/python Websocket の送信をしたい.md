---
type: note
---

#python #raspberry_pi 

---
2023-02-07  17:58

# python Websocket の送信をしたい

```shell
$ pip3 install websocket-client
```

example

```python
>>> import websocket
>>> ws = websocket.WebSocket()
>>> ws.connect("ws://people10.local:1880/ws-event/face")
>>> ws.send("Hello, Server")
>>>
>>> print(ws.recv())
>>> ws.close()
```