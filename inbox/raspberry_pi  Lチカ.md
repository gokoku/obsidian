#raspberry_pi 

---
2022-03-04  15:52

# raspberry_pi  Lチカ

基本からやってみる。


```python
import RPi.GPIO as GPIO
import time

pin = 24

GPIO.setmode(GPIO.BCM)
GPIO.setup(pin, GPIO.OUT)

try:
	while True:
		GPIO.output(pin, True)
		time.sleep(0.5)
		GPIO.output(pin, False)
		time.sleep(0.5)
except KeyboardInterrupt:
	pass
GPIO.cleanup()
```

setmode の GPIO.BCM はピン番号の解釈のしかた。

pin = 24 なら BCM では GPIO24 と書いてるところなので、分かりやすい。

## LED の R

ラズパイの3V の電圧は 3.3V くらい

LED の順方向電圧は 1.8V くらいらしい。

LED の電流は 4mA くらいにすると

(3.3 - 1.8) / (0.004) = 375 くらい。


##### 200Ω しかないぞ
7.5mAくらいだからいっか。

Raspberry PI の最大電流量は、**16mA** までとのこと。

