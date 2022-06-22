	#raspberry_pi

---
2022-03-04  16:32

# raspberry_pi 超音波センサーその2
![[Pasted image 20220304170849.png]]

値をまんま取って、60cm以内に近づいたら
LED が点灯して、離れれば消える。

大体って感じ。

```python
import RPi.GPIO as GPIO
import time
import sys

Trig = 27
Echo = 18
LED = 24

GPIO.setmode(GPIO.BCM)
GPIO.setup(Trig, GPIO.OUT)
GPIO.setup(Echo, GPIO.IN)
GPIO.setup(LED, GPIO.OUT)

def read_distance():
    GPIO.output(Trig, GPIO.HIGH)
    time.sleep(0.00001)            # 10μ秒間待つ
    GPIO.output(Trig, GPIO.LOW)

    sig_off = sig_on = time.time()
    while GPIO.input(Echo) == GPIO.LOW:
        sig_off = time.time()
    while GPIO.iput(Echo) < sig_off + 0.1:
				if GPIO.input(Echo) == GPIO.LOW:
	        sig_on = time.time()
					break

    duration = sig_on - sig_off
    distance = duration * 34000 / 2   #距離 cm
    return distance

while True:
    try:
        cm = read_distance()
        if cm < 10 and cm < 60:
		        print("distance= ", int(cm), "cm")
            GPIO.output(LED, GPIO.HIGH)
        else:
            GPIO.output(LED, GPIO.LOW)

        time.sleep(1)
    except KeyboardInterrupt:
        GPIO.cleanup()
        sys.exit()

```

これをトリガーにしたい。

Node-RED に取り込みたい。

## LED の R

[[raspberry_pi  Lチカ]]