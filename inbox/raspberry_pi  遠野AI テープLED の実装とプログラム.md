#raspberry_pi #python

---
2022-03-29  15:24

# raspberry_pi  遠野AI テープLED の実装とプログラム

## 実体配線図

![[Pasted image 20220329152430.png | 400]]

## Python プログラム

WS2815 を光らせるクラスを作って、ここで光り方のパターン化をするライブラリにした。

```python:ws2815_pattern.py

import math
import numpy as np
import time
from rpi_ws281x import Color, PixelStrip


LED_COUNT = 60       # Number of LED pixels
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
        color = Color(0, 0, 0)
        for i in range(self.__strip.numPixels()):
            self.__strip.setPixelColor(i, color)
            self.__strip.show()


    def blink(self, red: int, green: int, blue: int) -> None:
        for n in range(0, 200, 2):
            c = 100 - abs(n - 100)
            for i in range(0, LED_COUNT):
                r = c if red else 0
                g = c if green else 0
                b = c if blue else 0
                self.__strip.setPixelColor(i, Color(r, g, b))
            self.__strip.show()
            time.sleep(0.04)

    def point(self, direct: int, red: int, green: int, blue: int) -> None:
        if direct > 0:
            for ptr in range(0, LED_COUNT, direct):
                for i in range(0, LED_COUNT):
                    if i == ptr:
                        self.__strip.setPixelColor(i, Color(red, green, blue))
                    else:
                        self.__strip.setPixelColor(i, Color(0, 0, 0))

                self.__strip.show()
                time.sleep(0.01)
        else:
            for ptr in range(LED_COUNT+direct, 0+direct,  direct):
                for i in range(0, LED_COUNT):
                    if i == ptr:
                        self.__strip.setPixelColor(i, Color(red, green, blue))
                    else:
                        self.__strip.setPixelColor(i, Color(0, 0, 0))

                self.__strip.show()
                time.sleep(0.01)



    def wave(self, direct: int, red: int, green: int, blue: int) -> None:

        colors = [
            Color(round(red/10), round(green/10), round(blue/15)),
            Color(round(red/10), round(green/10), round(blue/10)),
            Color(round(red/6), round(green/6), round(blue/6)),
            Color(round(red/2), round(green/2), round(blue/2)),
            Color(red, green, blue),
            Color(red, green, blue),
            Color(round(red/4), round(green/4), round(blue/4)),
            Color(round(red/6), round(green/2), round(blue/6)),
            Color(round(red/8), round(green/8), round(blue/8)),
            Color(round(red/6), round(green/6), round(blue/10)),
            Color(round(red/10), round(green/10), round(blue/15)),
        ]

        if direct > 0:
            for ptr in range(0, LED_COUNT - len(colors), direct):
                for i in range(0, LED_COUNT):
                    if i >= ptr and i < ptr + len(colors):
                        self.__strip.setPixelColor(i, colors[i-ptr])
                    else:
                        self.__strip.setPixelColor(i, Color(0, 0, 0))
                self.__strip.show()
                time.sleep(0.05)

        else:
            for ptr in range(LED_COUNT - len(colors) - direct, 0 - direct, direct):
                for i in range(0, LED_COUNT):
                    if i >= ptr and i < ptr + len(colors):
                        self.__strip.setPixelColor(i, colors[i-ptr])
                    else:
                        self.__strip.setPixelColor(i, Color(0, 0, 0))
                self.__strip.show()
                time.sleep(0.05)
```


### 聴いてる状態を表すプログラム
```python:ws2815_listen.py
import math
import numpy as np
import time
from rpi_ws281x import Color, PixelStrip

import ws2815_pattern as WS


if __name__ == '__main__':

    led = WS.Ws281x()

    led.wave(1, 255, 0, 100)
    led.wave(-1, 155, 0, 150)
    led.wave(1, 0, 150, 255)
    led.wave(-1, 255, 0, 100)
    led.wave(1, 155, 0, 150)
    led.wave(-1, 0, 150, 255)

    time.sleep(1)
    led.off()

```

### クリアする
```python:ws2815_off.py
import math
import numpy as np
import time
from rpi_ws281x import Color, PixelStrip

import ws2815_pattern as WS


if __name__ == '__main__':

    led = WS.Ws281x()
    led.on(100,100,100)
    time.sleep(0.5)
    led.off()
```

### ワーニングパターン
今回は使ってない。
```python:ws2815_warning.py
import math
import numpy as np
import time
from rpi_ws281x import Color, PixelStrip

import ws2815_pattern as WS


if __name__ == '__main__':

    led = WS.Ws281x()

    led.point(1, 255, 0, 0)
    led.point(2, 255, 50, 0)
    led.point(3, 255, 100, 0)
    led.point(4, 255, 50, 0)
    led.point(-1, 255, 0, 0)
    led.point(-2, 255, 50, 0)
    led.point(-3, 255, 0, 0)
    led.point(-4, 255, 50, 0)
    led.point(1, 255, 0, 0)
    led.point(2, 255, 50, 0)
    led.point(3, 255, 100, 0)
    led.point(4, 255, 50, 0)
    led.point(-1, 255, 0, 0)
    led.point(-2, 255, 50, 0)
    led.point(-3, 255, 0, 0)
    led.point(-4, 255, 50, 0)

    time.sleep(1)
    led.off()

```

### welcome
今回は使ってない。
```python:ws2815_welcome.py
import math
import time
from rpi_ws281x import Color, PixelStrip

import ws2815_pattern as WS


if __name__ == '__main__':
    led = WS.Ws281x()

    for i in range(0, 2):
        led.blink(0, 0, 1)
        led.blink(0, 1, 1)
        led.blink(0, 1, 0)

    time.sleep(1)
    led.off()
```


# Shell Script
node-red で起動し、引数でパターンを切り替える。

```shell:ws2815.sh
#!/bin/bash

usage='usage: $ ws2815.sh [thinking | listen | warning ]'

if [ $# != 1 ]; then
    echo $usage
    exit 1
fi

if [ $1 == thinking ]; then
    sudo python3 /home/pi/AI/ws2815_led/ws2815_thinking.py

elif [ $1 == listen ]; then
    sudo python3 /home/pi/AI/ws2815_led/ws2815_listen.py

elif [ $1 == warning ]; then
    sudo python3 /home/pi/AI/ws2815_led/ws2815_warning.py
    sudo python3 /home/pi/AI/ws2815_led/ws2815_off.py
else
    echo $usage
fi

exit 0
```

プロセスを探し出して強制終了させる。
```shell:ws_kill.sh

#!/bin/bash

usage='usage: $ ws2815.sh [thinking | listen | warning ]'

if [ $# != 1 ]; then
    echo $usage
    exit 1
fi

if [ $1 == thinking ]; then
    sudo python3 /home/pi/AI/ws2815_led/ws2815_thinking.py

elif [ $1 == listen ]; then
    sudo python3 /home/pi/AI/ws2815_led/ws2815_listen.py

elif [ $1 == warning ]; then
    sudo python3 /home/pi/AI/ws2815_led/ws2815_warning.py
    sudo python3 /home/pi/AI/ws2815_led/ws2815_off.py
else
    echo $usage
fi

exit 0
```
