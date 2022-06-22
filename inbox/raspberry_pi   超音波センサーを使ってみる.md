#raspberry_pi 

---
2021-12-24

# 超音波センサ

HC-SR04

https://denkisekkeijin.com/raspberrypi/pi-ultrasound/

![30_回路図](https://denkisekkeijin.com/wp-content/uploads/2020/07/RPD035_pi_ultrasound_%E3%83%96%E3%83%AC%E3%83%83%E3%83%89%E3%83%9C%E3%83%BC%E3%83%89-1024x891.png)

HC-SR04 は 5v で駆動するので、出力が 5V で raspberry pi の 3.3V を壊す可能性があるので分圧してるところが味噌。


![[Pasted image 20211224171600.png]]

Trig で出力、Echo で反射音を受ける仕組みだ。

```python
import RPi.GPIO as GPIO
import time
import sys

Trig = 27
Echo = 18

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

while True:
    try:
        cm = read_distance()
        if cm > 2 and cm < 400:
           print("distance= ", int(cm), "cm")
        time.sleep(1)
    except KeyboardInterrupt:
        GPIO.cleanup()
        sys.exit()

```

結構うまくいった。

が、結構ガタガタ上下するようだ。

平滑化して距離だけで条件を分けるのがシンプルか。

距離が3mだったら軽く挨拶

1~2m だったらおすすめする。

1m 以内だったら、なんかお得情報みたいな、ちょっとシークレット機能のような。


## 移動平均で平滑化する

```python
#!/usr/bin/python3

import RPi.GPIO as GPIO
import time
import sys
import numpy as np
from collections import deque
import pickle
import os

sys.setrecursionlimit(10000) # エラー回避

Trig = 27
Echo = 17

GPIO.setwarnings(False)  # ワーニング回避
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

class DataQue:
    que = deque([], maxlen=30) # 移動平均に使うキュー

    def average(self, val):
        with open(os.path.dirname(__file__) + '/sonic_temp.txt', 'rb') as f:
            self.que = pickle.load(f)

        self.que.append(val)
        #t(self.que)

        with open(os.path.dirname(__file__) + '/sonic_temp.txt', 'wb') as f:
            pickle.dump(self.que, f)

        return np.average(self.que)


cm = round(read_distance(),2)
if cm < 2 or cm > 150:
    cm = 0

data_que = DataQue()

x = round(data_que.average(cm), 1)

print(x, end='')

GPIO.cleanup()

```
Node-RED で 0.2 sec おきにトリガーがかかるようにした。

![[Pasted image 20211228181027.png]]

![[Pasted image 20211228181157.png]]

これと
