#android

---
2021-10-21

# Android Studio の import TensorFlow Lite model

File -> New -> Other -> TensorFlow Lite Model

![[Pasted image 20211021151048.png]]

.tflite ファイルを予め DL して import するみたいだ。

https://tfhub.dev/tensorflow/lite-model/ssd_mobilenet_v1/1/metadata/2

![[Pasted image 20211021151436.png]]

ここで DL した lite-model_ssdd...tlfile の行き先↓

app/src/main/ml/lite-model_ssd_mobilenet_v1_1_metadata_2.tlfile


https://developer.android.com/studio/releases/#4.1-tensor-flow-lite-models

使い方があるが、よくわからん。more infomation

こんなに簡単に使えるよ的なのだが、も少し具体例を見せてくれ。

![Screenshot of TensorFlow Lite model viewer](https://developer.android.com/studio/images/write/tf-lite-model-viewer.png)

ここのTFLite ファイルを使ってる。meta データ入りのデータの使い方知りたい。

てゆーかこれ、classify 用のやつじゃね?

https://tfhub.dev/tensorflow/lite-model/mobilenet_v1_0.25_160_quantized/1/metadata/1


