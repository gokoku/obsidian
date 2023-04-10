---
type: note
---

#ai

---
2022-08-24  13:15

# ai. StableDiffusion ってすごい?

https://note.com/fladdict/n/n13c1413c40de

OSS 公開ってどんなんだ?
あわよくば、ローカルマシンでできちゃうのか??

「アート特化のMidJourneyのほうが雑なプロンプトでもよい画像を出す。」というのもあるらしい。


## Github

https://github.com/CompVis/stable-diffusion

### conda をセットアップする。

```shell
$ asdf plugin add python
どれ ? anaconda anaconda2 anaconda3 一杯ある!

どうも、python3 なら anaconda3 らしいが、anaconda で

$ asdf install python anaconda-4.0.0
$ asdf global python anaconda-4.0.0
$ asdf reshim python

$ conda --version
```

### stable_diffusion のセットアップ

A suitable conda environment named ldm can be created and activated with: 
とある。
すでにstable-diffusion には ldm が入ってる。
 
```shell
$ git clone git@github.com:CompVis/stable-diffusion.git
$ cd stable-diffusion
$ conda env create -f environment.yaml

$ conda activate ldm
```

asdf 環境のせいか上手くいかず。

## Docker の記事がある

https://zenn.dev/hayatok/articles/6141a9a46e4f48

事前に Hugging Face のアカウントを作って Token を作る必要ある。
ここにモデルが置いてあるようだ。

https://huggingface.co/

token を作ってコピーしとく。

`~/docker/stable-diffusion/Dockerfile`
```dockerfile
FROM nvcr.io/nvidia/cuda:11.7.1-cudnn8-runtime-ubuntu20.04

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Tokyo

RUN apt-get update && apt-get install -y wget git git-lfs libglib2.0-0 libsm6 libxrender1 libxext-dev

RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    sh Miniconda3-latest-Linux-x86_64.sh -b -p /opt/miniconda3 && \
    rm -r Miniconda3-latest-Linux-x86_64.sh

ENV PATH /opt/miniconda3/bin:$PATH

RUN git clone https://github.com/CompVis/stable-diffusion && \
    cd stable-diffusion && \
    conda init bash && \
    conda env create -f environment.yaml && \
    echo "conda activate ldm" >> ~/.bashrc
```

```shell
$ cd ~/docker/stable-diffusion
$ docker build -t stable:0.1 .
```

mac では動かん。

## Google Colaborator

https://kasata.medium.com/2022-google-colab%E3%81%A7stable-diffusion%E3%82%92%E5%8B%95%E3%81%8B%E3%81%99%E6%96%B9%E6%B3%95-3daf167aaa3c

google drive console から新規-->その他-->google colaborator

https://colab.research.google.com/github/huggingface/notebooks/blob/main/diffusers/stable_diffusion.ipynb#scrollTo=xSKWBKFPArKS

ここにあるように動かしてみた。

設定から GPU にしないと進めないので注意。

```PYTHON

!nvidia-smi


!pip install diffusers==0.2.4
!pip install transformers scipy ftfy
!pip install "ipywidgets>=7,<8"





from google.colab import output
output.enable_custom_widget_manager()





from huggingface_hub import notebook_login
notebook_login()





import torch
from torch import autocast
from diffusers import StableDiffusionPipeline

  

model_id = "CompVis/stable-diffusion-v1-4"
device = "cuda"

  
  

pipe = StableDiffusionPipeline.from_pretrained(model_id, use_auth_token=True)
pipe = pipe.to(device)

  

prompt = "a photo of an astronaut riding a horse on mars"
with autocast("cuda"):
image = pipe(prompt, guidance_scale=7.5)["sample"][0]
image.save("astronaut_rides_horse.png")
```


![[Pasted image 20220825181520.png]]
![[Pasted image 20220825181533.png]]
![[Pasted image 20220825181552.png]]

![[Pasted image 20220825181609.png]]

![[Pasted image 20220825181852.png]]

 ディスクに生成された。
![[スクリーンショット 2022-08-25 18.19.23.png]]

![[Pasted image 20220825182012.png]]

ファイルは Google Drive に保存される。
ランタイムは終わったら接続を切る。
どっちにしろブラウザ閉じたら90分で切られる。

後日再びやるには最初から実行していく。

キャッシュクリアしたいけど、?