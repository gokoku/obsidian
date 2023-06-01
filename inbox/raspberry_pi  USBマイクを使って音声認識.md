#raspberry_pi 

---
2021-05-31

# USBマイクを Raspberry Pi で使う

https://qiita.com/mininobu/items/f181b762955932cec6d6

```shell
$ lsusb

Bus 001 Device 006: ID 0c76:161e JMTek, LLC.

```

```shell
$ arecord -l
**** ハードウェアデバイス CAPTURE のリスト ****
カード 2: Device [USB PnP Audio Device], デバイス 0: USB Audio [USB Audio]
  サブデバイス: 1/1
  サブデバイス #0: subdevice #0
```

card : 2

device: 0

とのことで、録音してファイルに保存する
```shell
$ arecord -D plughw:2,0 test.wav

$ aplay text.wav
```
録音された。


### plughw:2,0 can't open
```shell
$ arecord -l

**** ハードウェアデバイス CAPTURE のリスト ****
カード 1: Device [USB PnP Audio Device], デバイス 0: USB Audio [USB Audio]
  サブデバイス: 1/1
  サブデバイス #0: subdevice #0
```

カードが 3になってた。
USBポートが口の場所関係なく、変わる。

この場合は plughw:1,0 とか書き換える必要がある。


## 初期設定がしたい

スピーカーも、マイクも、メニューだけで設定すると、切り替わってたりして厄介だ。

### USB オーディオの優先順位を上げる

```shell
$ aplay -l
$ arecord -l

これで、マイクが カード 2
スピーカが 3 みたいだ
```

~/.asoundrc に設定してたようだ。
これは raspi-config での設定らしい。
/usr/bin/raspi-config に設定ファイルがある。

```shell:~/.asoundrc
pcm.!default {
  type asym
  playback.pcm {
    type plug
    slave.pcm "output"
  }
  capture.pcm {
    type plug
    slave.pcm "input"
  }
}

pcm.output {
  type hw
  card 3
}

pcm.input {
  type hw
  card 2
}

ctl.!default {
  type hw
  card 0
}
```
これでいいのかな。

何だか良さげ。



## 録音する

```shell:voice_rec.sh

#!/bin/bash

# 5秒間録音
# 3秒の無音でストップ

sox -t alsa plughw:2,0 -r 44100 /home/pi/AI/watson/voice.wav trim 0 4 silence 0 1 2.0 -35d
```



