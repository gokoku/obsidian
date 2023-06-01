#ai

---
2021-10-21

# SSD (Single Shot Multibox Detector)

複数検知手法の一つ。
YOLO , SSD , Faster R-CNN

![](https://sorabatake.jp/wp-content/uploads/2020/11/image1-3.png)

### IoU ( Intersection over Union) 

一つの物体に複数のボックスが重なるので、IoU を使って除去する。

除去操作を Non-maximum suppression という。

IoU = Intersection(重なりの大きさ) / Union (合わせた大きさ)

![how to calculate IoU](https://www.acceluniverse.com/blog/developers/7e7eee050e70f4287da42c41a8e0383da39b6c63.png)


### モデルつくるっすか??!!

SSDを用いて飛行機の物体検出にチャレンジしてみた

https://sorabatake.jp/16185/


