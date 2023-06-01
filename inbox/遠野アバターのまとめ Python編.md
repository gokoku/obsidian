---
type: note
---

#python #raspberry_pi 

---
2023-02-09  17:44

# 遠野アバターのまとめ Python編

![[Pasted image 20230209174430.png]]

## 正面を検知させる

顔の四角の幅と鼻が中央から離れている度合いで判定する。

![[tono-avator 2023-02-09 17.48.38.excalidraw | 800]]


```python
!/usr/bin/env python
# -*- coding: utf-8 -*-
import copy
import time
import cv2
from picamera2 import Picamera2
import websocket

from yunet.yunet_onnx import YuNetONNX


# websocket set
ws = websocket.WebSocket()
ws.connect("ws://people10.local:1880/ws-event/face")

def main():
    count_max = 3

    model_path = 'model/face_detection_yunet_120x160.onnx'
    input_shape = (160,120)
    score_th = 0.6
    nms_th = 0.3
    topk = 5000
    keep_topk = 750

    # preparate picamera2
    picam2 = Picamera2()
    config = picam2.create_preview_configuration(main={"format":'XRGB8888', "size": (640, 480)})
    picam2.configure(config)
    picam2.start()

    # load Models
    yunet = YuNetONNX(
        model_path=model_path,
        input_shape=input_shape,
        conf_th=score_th,
        nms_th=nms_th,
        topk=topk,
        keep_topk=keep_topk,
    )

    count = 0
    while True:

        start_time = time.time()

        # capture
        frame = picam2.capture_array()
        image = copy.deepcopy(frame)
        # detect
        bboxes, landmarks, scores = yunet.inference(frame)
        elapsed_time = time.time() - start_time
        # draw image
        display_image = draw_debug(
            image,
            score_th,
            input_shape,
            bboxes,
            landmarks,
            scores,
        )
        time.sleep(0.01)
        # exit ESC
        key = cv2.waitKey(1)
        if key == 27:  # ESC
            ws.close()
            break

        # diaplay image
        debug_image = cv2.resize(display_image, dsize=None, fx=0.3, fy=0.3)
        count += 1
        if count > count_max:
            cv2.namedWindow("AI", cv2.WINDOW_GUI_NORMAL)
            cv2.resizeWindow('AI', 220, 165)
            cv2.imshow('AI', debug_image)
            cv2.moveWindow('AI', 0, 600)
            count = 0

    cv2.destroyAllWindows()

def draw_debug(
    image,
    score_th,
    input_shape,
    bboxes,
    landmarks,
    scores,
):
    image_width, image_height = image.shape[1], image.shape[0]
    debug_image = copy.deepcopy(image)

    for bbox, landmark, score in zip(bboxes, landmarks, scores):
        if score_th > score:
            continue

        # face bounding box
        x1 = int(image_width * (bbox[0] / input_shape[0]))
        y1 = int(image_height * (bbox[1] / input_shape[1]))
        x2 = int(image_width * (bbox[2] / input_shape[0])) + x1
        y2 = int(image_height * (bbox[3] / input_shape[1])) + y1



        # score
        cv2.putText(debug_image, '{:.3f}'.format(score), (x1, y1-10),
                   cv2.FONT_HERSHEY_DUPLEX, 2.0, (255, 255, 0), 3)

        # point in face
        for _, landmark_point in enumerate(landmark):
            x = int(image_width * (landmark_point[0] / input_shape[0]))
            y = int(image_height * (landmark_point[1] / input_shape[1]))
            cv2.circle(debug_image, (x, y), 5, (255, 255, 0), -1)
        # nose
        x_nose = int(image_width * (landmark[2][0] / input_shape[0]))
        y_nose = int(image_height * (landmark[2][1] / input_shape[1]))
        cv2.circle(debug_image, (x_nose, y_nose), 10, (0, 0, 255), -1)

        # judgment front view
        ave = round((x2 + x1) / 2)
        dx = abs(ave - x_nose)
        ratio = round(dx / ave * 100)

        color = (255,255,0)
        if ratio < 8 and (x2 - x1) > 220:
            color = (0, 50, 255)
            ws.send('{"mode": 0}')
            time.sleep(1)
            print("↑ Hit!!\n")

        cv2.rectangle(debug_image, (x1, y1), (x2, y2), color, 5)
    return debug_image


if __name__ == '__main__':
    main()

```