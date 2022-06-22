#raspberry_pi 

---
2021-12-29

# LED テープを光らせる


![[Pasted image 20211229101703.png]]


### WS2815 <--> raspberry pi

https://dev.classmethod.jp/articles/raspberry-pi-tape-led-ws2815/

#### ライブラリのインストール

```shell
$ sudo pip3 install rpi_ws281x
```

sudo で入れる。

raspberry pi 4 では、 **GPIO21** が PCM_DOUT らしい。てゆーか、これしか上手くいかなかった。


led_ws2815.py

```python

from rpi_ws281x import Color, PixelStrip

LED_COUNT = 30       # Number of LED pixels
LED_PIN = 21         # GPIO pin connected to the pixels (must support PWM)
LED_FREQ_HZ = 800000 # LED signal frequency in hertz (usually 800kHz)
LED_DMA = 10         # DMA channel to use for generating signal (try 10)
LED_BRIGHTNESS = 255 # Set to 0 for darkest and 255 for brightest
LED_INVERT = False   # True to invert the signal
LED_CHANNEL = 0
LED_STATE_OFF = Color(0, 0, 0) # OFF


class Ws281x:
    def __init__(self):
        self.__strip = PixelStrip(
            LED_COUNT,
            LED_PIN,
            LED_FREQ_HZ,
            LED_DMA,
            LED_INVERT,
            LED_BRIGHTNESS,
            LED_CHANNEL,
        )
        self.__strip.begin()

    def on(self, red: int, green: int, blue: int) -> None:
        color = Color(red, green, blue)
        for i in range(self.__strip.numPixels()):
            self.__strip.setPixelColor(i, color)
            self.__strip.show()

    def off(self) -> None:
        for i in range(self.__strip.numPixels()):
            self.__strip.setPixelColor(i, LED_STATE_OFF)
            self.__strip.show()

led = Ws281x()
led.on(127, 0, 0)
```

```shell
$ sudo python3 led_ws2815.py
```
sudo じゃないとエラーになる。

LED_COUNT で光らせるLEDの数を指定出来る。

何番目のLED かを指定してカラーを設定する。

一度パルスを送ったら LED はそれを保持してるようだ。

参考デモ

https://github.com/hajimef/ws2815test_rpi


## Signal を詰める

https://github.com/jgarff/rpi_ws281x

PWM と PCM と SPI  のどれかを選べるようだ。

ちゃんと LED をコントロール出来て、消え残りとかなく、

他のセンサーも使えるのかが知りたい。

ハードウェアPWM はヘッドフォン端子から音声を出力するものと併用は出来ないとのこと。

ということで、21 を使うことにした。

#### 回路

* レベルコンバータはかましてラズパイを守る
* GND を全部共通にすること。これでかなりハマってた。

12 Vアダプタと、レベルコンバータの GND 二つを全部繋ぐことで、ラズパイからのコントロールがバッチリ効くようになった。

と思ったら、まだ安定してなかった。

**400Ω をシグナルに噛ませたら本当に安定した**

確かに電圧が Low でも高かった。

![[Pasted image 20220329152155.png]]

