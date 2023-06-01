#python 

```bash
$ pip3 install websocket-client
```

raspberry pi は sudo をつけないと、ローカルユーザーに入る。

ローカルユーザは USB メモリでマウントしてある。

https://pypi.org/project/websocket_client/

ただ一言send したいとき、

```bash
import websocket

ws = websocket.WebSocket()
ws.connect("ws://localhost:1880/ws-event")
ws.send("images/mv01.webm")
ws.close()
```

---

Long live Connect の方法

```bash
import websocket

try:
  import thread
except ImportError:
  import _thread as thread
import time

class Websocket_Client():
  def __init__(self, host_addr):

    websocket.enableTrace(True)

    self.ws = websocket.WebSocketApp(host_addr,
      on_message = lambda ws, msg: self.on_message(ws, msg),
      on_error = lambda ws, msg: self.on_error(ws, msg),
      on_close = lambda ws: self.on_close(ws))
    self.ws.on_open = lambda ws: self.on_open(ws)

  def on_message(self, ws, message):
    print("receive : {}".format(message))

  def on_error(self, ws, error):
    print(error)

  def on_close(self, ws):
    print("### closed ###")

  def on_open(self, ws):
    thread.start_new_thread(self.run, ())

  def run(self, *args):
    while True:
      time.sleep(0.1)
      input_data = input("send data: ")
      self.ws.send(input_data)

    self.ws.close()
    print("thread terminating...")

  def run_forever(self):
    self.ws.run_forever()

if __name__ == "__main__":
  HOST_ADDR = "ws://localhost:1880/ws-event"
  ws_client = Websocket_Client(HOST_ADDR)
  ws_client.run_forever()
```

```bash
$ python3 ws_client.py

send data: images/mv01.webm
```

node-red で ws://localhost:1880/ws-event を開けてるので、飛んでいく!

---

