#python


Raspberry pi で python3 を使うならシステムにあるものがスムーズだった。

pyenv を使うと apt の恩恵が受けられなかった。

```bash
$ sudo apt python3-opencv
```

これだけ。

tensorflow のサンプルをダウンロードする。

```bash
$ git clone https://github.com/tensorflow/tensorflow.git
$ cd tensorflow/tensorflow
```

## 物体検出サンプル

```bash
$ cd examples/object_detect/raspberry_pi
$ bash download.sh ./downloads

$ python3 detect_picamera.py --model=./downloads/detect.tflite --label=./downloads/coco_labels.txt
```

![](image-kn8aeilk.png)

---

## Tensorflow Lite とは

Tensorflow の軽量インタプリタ

[Python quickstart | TensorFlow Lite](https://tensorflow.org/lite/guide/python)

ここから ARM 32 python3.7 のURLをコピーして pip でインストールする。

```bash
$ pip3 install https://dl.google.com/coral/python/tfite_runtime-2.1.0.....
```