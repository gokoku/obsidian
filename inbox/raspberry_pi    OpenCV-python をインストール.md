#raspberry_pi  #python 

---
2021-11-12

# Raspberry pi に openCV-python をインストール

https://raspi-katsuyou.com/index.php/2020/06/30/15/50/08/679/

```shell
$ sudo apt update && sudo apt upgrade
$ sudo apt install libhdf5-dev libhdf5-serial-dev libhdf-103
$ sudo apt-get install libqtgui4 libqtwebkit4 libqt4-test python3-pyqt5
$ sudo apt-get install libatlas-base-dev
$ sudo apt-get install libjasper-dev


$ pip3 install --upgrade setuptools
```


```shell
$ pip3 install opencv-python
```

これだと、2,3時間して失敗する。

```shell
$ pip3 install opencv-python==4.5.1.48
```

このバージョンで動いた。サクッと入る。

```shell
$ python3
>>> import cv2
>>> cv2.__version__
'4.5.1'
```

