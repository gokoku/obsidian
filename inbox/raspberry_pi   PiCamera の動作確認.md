#raspberry_pi 


https://dream-soft.mydns.jp/blog/developper/smarthome/2020/01/649/


```bash
$ sudo apt update

$ sudo apt install python3-opencv
```

raspberry pi を使うなら system の python3 が apt の恩恵が受けられる。

この恩恵はかなりのものだった。

.bashrc

```bash
export DISPLAY=:0.0
```

```bash
$ python3
>> import picamera
>> camera = picamera.PiCamera()
>> camera.start_preview()

>> camera.stop_preview()
```

インタプリタだけで確認は出来る。

