#command #mqtt

---
2021-12-10

# Mac に Mosquitto を入れて MQTTする

```shell
$ brew install mosquitto




サーバ立ち上げ
$ brew services restart mosquitto


購読
$ moszuitto_sub -t myTopic
```
myTopic トピックの購読。

セキュリティーソフトにはじかれた。

