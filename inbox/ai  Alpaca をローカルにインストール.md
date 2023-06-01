---
type: note
---

#ai

---
2023-03-22  14:53

# ai  Alpaca をローカルにインストール

https://gigazine.net/news/20230320-chat-ai-alpaca-cpp/

https://github.com/antimatter15/alpaca.cpp

```shell
$ git clone https://github.com/antimatter15/alpaca.cpp.git
```

てゆーか、バイナリーあった。
Get Started(7B) に latest release がある。

Download で hagface からモデルをダウンロードして、同じディレクトリーに置くと動く。

```shell
$ ./chat_mac

> Hello
```

めちゃくちゃcpuリソース食ってーの遅い応答。数10秒。

遅すぎる。GPU 必須じゃね?!

