#js/ml5 


ここで機械学習のClassify のモデルデータを簡単に作れて、ダウンロードしたり、リンクしたりして使うことができる優れたサービス。

機械学習の敷居を下げてくれる。

![](image-kndvo9ba.png)

![](image-kndvogxz.png)

この絵でラズパイで chromium で画像トレーニングした。

4種類、weather , oracle , sale , news

![](image-kndvotiy.png)

[https://teachablemachine.withgoogle.com/models/pOq1c0KIs](https://teachablemachine.withgoogle.com/models/pOq1c0KIs)

ここにアップロードした。

![](image-kndvpaba.png)

カメラに絵を見せると90%で特定できてる。

モデルのexport を Tensorflow Lite にすると、全部ダウンロードになる。

よくわからないので、EdgeTPU にしてダウンロードをクリックした。

10分くらいかかったかも。

converted_edgetpu.zip がダウンロードされた。

![](image-kndvpmbs.png)

