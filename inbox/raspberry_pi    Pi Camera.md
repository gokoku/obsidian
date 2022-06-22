#raspberry_pi 

```shell
$ sudo raspi-config
```

Pi Camera enabled

確認
```shell
$ sudo vcgencmd get_camera
supported=1
detected=1

$ sudo raspistill -o test.jpg
```