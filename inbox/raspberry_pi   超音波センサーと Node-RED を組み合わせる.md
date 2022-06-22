#raspberry_pi 

---
2022-03-07  15:53

# raspberry_pi   超音波センサーと Node-RED を組み合わせる

python スクリプトは1回限りのワンショットで距離を出す。

python スクリプトを使うのは、Node-RED ではこの細かい秒で計測する精密さに欠けてダメらしいとのこと。

https://blog.tacck.net/archives/1192




<div class="rich-link-card-container"><a class="rich-link-card" href="https://blog.tacck.net/archives/1192" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://blog.tacck.net/wp-content/uploads/2020/12/AdobeStock_235324116.jpeg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">クリスマスの定番（？） Raspberry Pi と超音波センサーでクリスマスツリーを光らせて SORACOM Harvest Data でデータ収集 | tacckの積み重ねるブログ</h1>
		<p class="rich-link-card-description">
		メリークリスマス!アドベントカレンダーも最終日のクリスマス、そしてクリスマスと言えばラズパイですよね! ということで、 ゆるWeb勉強会@札幌 Advent Calendar 2020 の最後は、超音波センサーを使ってク […]
		</p>
		<p class="rich-link-href">
		https://blog.tacck.net/archives/1192
		</p>
	</div>
</a></div>


これに合わせて python script を改良した。

```python
!/usr/bin/python3

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
    while GPIO.input(Echo) < sig_off + 0.1:
        if GPIO.input(Echo) == GPIO.LOW:
           sig_on = time.time()
           break
        
    duration = sig_on - sig_off
    distance = duration * 34000 / 2   #距離 cm
    return distance

while True:
    try:
        cm = read_distance()

        if cm > 2 and cm < 60:
            print("distance= ", int(cm))
            GPIO.output(LED, GPIO.HIGH)
        else:
            GPIO.output(LED, GPIO.LOW)

        time.sleep(1)
    except KeyboardInterrupt:
        GPIO.cleanup()
        sys.exit()


```

これをNode-RED でtimestamp で繰り返し呼び出すと、スクリプトのタイミングで重なって呼び出して、GPIO がバッティングしてると警告される。
なんとも不安定なシステムになる。
呼び出してからrhl2スクリプトは1秒sleep してるから呼び出しも 1秒以上にすれば取れないこともない。てゆーか取れる。

あんまりモッサイ動きにしたくないのだが。

この記事では、自作のノードを作ってる。
その道を行ってみるか?? 

赤外線センサーで誤魔化してみるか??

