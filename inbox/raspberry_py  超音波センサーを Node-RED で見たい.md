#raspberry_pi  #node-red 

---
2021-12-27

#  超音波センサーを Node-RED で見たい

#raspberry_pi   超音波センサーを使ってみる

Trig を GPIO27 からレベルコンバータで 5Vにしたラズパイからの出力。

Echo を GPIO17 からレベルコンバータで 3.3V にしてラズパイに入力。

Python で動かして値を標準出力にして Node-RED で取得する。


python コード

```python3
#!/usr/bin/python3
 
import RPi.GPIO as GPIO
import time
import sys
 
Trig = 27
Echo = 17
 
GPIO.setmode(GPIO.BCM)
GPIO.setup(Trig, GPIO.OUT)
GPIO.setup(Echo, GPIO.IN)

def read_distance():
    GPIO.output(Trig, GPIO.HIGH)
    time.sleep(0.00001)            # 10μ秒間待つ
    GPIO.output(Trig, GPIO.LOW)

    sig_off = sig_on = time.time()
    while GPIO.input(Echo) == GPIO.LOW:
        sig_off = time.time()
    while GPIO.input(Echo) == GPIO.HIGH:
        sig_on = time.time()

    duration = sig_on - sig_off
    distance = duration * 34000 / 2   #距離 cm
    return distance

cm = read_distance()
if cm > 2 and cm < 400:
    print(cm)
else:
    print(-1)

GPIO.cleanup()
```

直の中に入らないときは negative


