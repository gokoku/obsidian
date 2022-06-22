#raspberry_pi 

---
2022-03-15  10:50

# raspberry_pi 焦電赤外線センサーのライブラリ

MotionSensor なるものがそれらしい。

https://www.denshi.club/parts/2021/05/gpiozero13import-mcp3001mcp3002mcp3004mcp3008-2.html

```
class gpiozero.MotionSensor( pin, queue_len=1, sample_rate=10, threshold=0.5, partial=False, pin_factory=None)
```

pin : センサのGPIOピン

センサー出力とPINの間に 10kΩを入れた。2v以下になったのでこれで行こう。

```python
import RPi.GPIO as GPIO
from gpiozero import MotionSensor
from time import sleep
import sys

PIN = 23
LED = 24

GPIO.setmode(GPIO.BCM)
GPIO.setup(PIN, GPIO.IN)
GPIO.setup(LED, GPIO.OUT)

sensor = MotionSensor(PIN)

def detected():
    print("Intruder Alert!")
    GPIO.output(LED, GPIO.HIGH)
    sleep(5)
    GPIO.output(LED, GPIO.LOW)

sensor.when_motion = detected

while True:
    try:
        print('.')
        sleep(1)
    except KeyboardInterrupt:
        GPIO.cleanup()
        sys.exit()
```

when_motion はスレッドっぽい。



### youtube のやつ
これがしっかり動く感じ。
https://www.youtube.com/watch?v=Tw0mG4YtsZk

wait_for_motion(timeout) は、active になるまで止まってるようだ。
ゲートだ。

```python
from gpiozero import MotionSensor
from gpiozero import LED

import sys

PIN = 23

led = LED(24)
pir = MotionSensor(PIN)

led.off()
while True:
    try:
        pir.wait_for_motion()
        print("Motion Detected")
        led.on()
        pir.wait_for_no_motion()
        print("Motion Stopped")
        led.off()

    except KeyboardInterrupt:
        GPIO.cleanup()
        sys.exit()
```



