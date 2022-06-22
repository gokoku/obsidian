#raspberry_pi 

---
2021-12-17

# 焦電赤外線センサーを動かす

https://portaltan.hatenablog.com/entry/2018/05/16/094219


詳しいデータ
https://www.kumikomi-kaihatu.com/technical-column/column-022/


SB612A

結論、人が入ってくるのと、出ていくのと区別が付かない。

トリガーにすると 去る時にも入ってしまう。

センサー出力とPINの間に 10kΩを入れた。2v以下になったのでこれで行こう。

![[Pasted image 20211217134808.png]]

### 確認

```shell

## GPIO設定
sudo echo 18 > /sys/class/gpio/export                #18ピンを使うよ
sudo echo in > /sys/class/gpio/gpio18/direction      # 入力だよ

### 確認
## 検知していない時
cat /sys/class/gpio/gpio18/value                     # 入力ないよ
0

## 検知中
cat /sys/class/gpio/gpio18/value
1                                                    # 入力あったよ
```

Lチカ確認

```shell
$ sudo echo 6 > /sys/class/gpio/export
$ sudo echo out > /sys/class/gpio/gpio6/direction
$ sudo echo 1 > /sys/class/gpio/gpio6/value         # チカ!!
$ sudo echo 0 > /sys/class/gpio/gpio6/value         # 消え
```

detect-sensor.py

```python
import RPi.GPIO as GPIO
from time import sleep
import sys

PIN = 23
LED = 24

GPIO.cleanup()

def init():
    GPIO.setmode(GPIO.BCM)
    GPIO.setwarnings(False)
    GPIO.setup(LED, GPIO.OUT)
    GPIO.setup(PIN, GPIO.IN, pull_up_down=GPIO.PUD_UP)
    print("--------------------")

def main():
    try:
        while True:
            value = GPIO.input(PIN)
            if value != 0:
                GPIO.output(LED, GPIO.HIGH)
                sleep(2)
                print ("Motion Detected")
            else:
                GPIO.output(LED, GPIO.LOW)
                sleep(2)
                print (".")
    except KeyboardInterrupt:
        GPIO.output(LED, GPIO.LOW)
        GPIO.cleanup()
        sys.exit()


init()
main()
```

センサーのツマミ

SENS : 感度             S   
DELAY : 保持時間    T  絞る方向で最小 2sec くらいになる


### 制御方法について

直にバスを見にいくのではなく、仮想ファイルシステムを読み書きして制御するとのこと。

https://tool-lab.com/raspi-gpio-controlling-command-1/

場所は /sys/class/gpio

##### 使う GPIO ピンの宣言
sudo echo 18 > /sys/class/gpio/export

こうすると gpi18 ファイルが作られるとのこと。

##### このピンが出力なのか入力なのかの指定

sudo echo in > /sys/class/gpio/gpip18/direction


##### 読み 書くときは echo

sudo cat < /sys/class/gpio/gpio18/value


##### 使い終わりの宣言

sudo echo 18 > /sys/class/gpio/unexport

## LED の R

[[raspberry_pi  Lチカ]]


# Node-RED でダッシュボード化して見たい
#raspberry_pi  焦電赤外線センサーを Node-RED で見たい

![[Pasted image 20220315173613.png]]

たったこれだけで オン・オフでLED が着いたり消えたりする。
たったこれだけ!?
部品は抵抗しか使ってない。