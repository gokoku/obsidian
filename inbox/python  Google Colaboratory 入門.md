#python #colaboratory


[https://qiita.com/shoji9x9/items/0ff0f6f603df18d631ab](https://qiita.com/shoji9x9/items/0ff0f6f603df18d631ab)

## 90分12時間ルール

新しいノートブックを開くと新しいインスタンスが立ち上がる。

ブラウザを閉じると**90分**後にインスタンスが破棄される。

開きっぱなしでも **12時間後**にインスタンスが破棄される。

```bash
あと何分
**$** !cat /proc/uptime | awk '{print $1 /60 /60 /24 "days (" $1 / 60 / 60 "h)"}'
```

[https://colab.research.google.com/notebooks/welcome.ipynb?hl=ja](https://colab.research.google.com/notebooks/welcome.ipynb?hl=ja)

![](image-kn9mdn5g.png)

Colabnotebook とな？ jupyter notebook かな？

新規をチェックしたらその他からの Google Colaboratory で nodebook が作られる。

![](image-kn9me7bq.png)

あとはJupyter notebook と変わらないね。

![](image-kn9mewus.png)


# 直打ち command line
```shell
! pwd
```


# Google Drive をマウントするには

例えば、Google Drive の マイドライブに jupyter_notebook/Math-for-Programmers-master があって、これをマウントしたい。

```python
from google.colab import drive
drive.mount('/content/drive')
```

ここで"マイドライブ/jupyter_notebook/Math-for-Programmers-master/Chapter 03/draw3d.py"
を import するには

```python
import sys
sys.path.append('/content/drive/MyDrive/jupyter_notebook/Math-for-Programmers-master/Chapter 03')
from draw3d import *
```



<div class="rich-link-card-container"><a class="rich-link-card" href="https://neptune.ai/blog/google-colab-dealing-with-files" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://neptune.ai/wp-content/uploads/2020/10/blog_feature_image_044664_7_3_8_3.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">How to Deal With Files in Google Colab: Everything You Need to Know - neptune.ai</h1>
		<p class="rich-link-card-description">
		Google Colaboratory is a free Jupyter notebook environment that runs on Google’s cloud servers, letting the user leverage backend hardware like GPUs and TPUs. This lets you do everything you can in a Jupyter notebook hosted in your local machine, without requiring the installations and setup for hosting a notebook in your local machine. Colab…
		</p>
		<p class="rich-link-href">
		https://neptune.ai/blog/google-colab-dealing-with-files
		</p>
	</div>
</a></div>

## ファイルの扱い方

https://neptune.ai/blog/google-colab-dealing-with-files

python から upload できるらしい。
中身は key value 
```python
uploaded = files.upload()
```

```python
df4 = pd.read_json("News_Category_dataset_v2.json", lines=True)
```
ファイルのダウンロード
```python
files.download('/content/sample_data_california_housing_train.csv')
```

