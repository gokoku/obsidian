#raspberry_pi 

```bash
$ git clone https://github.com/tensorflow/examples --depth 1

$ cd examples/lite/examples/image_classification/raspberry_pi

$ bash download.sh tmp

$ python3 classify_picamera.py --model tmp/mobilenet_v1_1.0_224_quant.tflite --labels tmp/labels_mobilenet_quant_v1_224.txt
```

