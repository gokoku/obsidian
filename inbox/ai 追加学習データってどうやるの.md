---
type: note
---

#ai

---
2023-02-16  09:38

# ai 追加学習データってどうやるの?

https://www.youtube.com/watch?v=omOlqAsBKfE

Stable Diffusion web UI で
Estra network で追加学習データを追加するとのこと。
追加学習 Extra network に LoRA がある。
これを Stable Diffusion web UI にセットしてキーワードで呼び出して使うらしい。



CIVIT AI
HAGING FACE

多分、フォローしてる 852話さんは Stable Diffusion Web UI を使ってて、LoRA 追加学習モデルを追加してるの....あれ? 自キャラをそのまま LoRA で追加学習とあるぞ??

自分で差分学習出来る世の中なのか?!!

https://note.com/852wa/n/n22c877b403f0

LoRA で30枚追加学習されるとあるぞ。

ckpt は AIデータモデルファイル

結局わからなくなった。

やりたいのは、自分のタッチを使ってみた事ない画像を作るなのかな。
だとすると、img 2 img 系でありかな。
自分のラフをモデルのタッチに変換するとかなのか?
とすると、WebUI で簡単に追加してた追加学習モデルを claboratry でどうやるのか。

いや、LoRA で追加学習させるとあるのは、どうやってやるのかかな。
Lora は GPUメモリが非力でも学習させることが出来る手法とのこと。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://note.com/npaka/n/ndb287a48b682" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://assets.st-note.com/production/uploads/images/93573395/rectangle_large_type_2_491194472fbe2df19557c4c1aba0f08d.png?fit=bounds&quality=85&width=1280')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Google Colab ではじめる LoRA｜npaka｜note</h1>
		<p class="rich-link-card-description">
		Google Colab で LoRA を試したのでまとめました。  1. LoRA  「LoRA」(Low-rank Adaptation)は、数枚の被写体画像と対応するテキストを元にファインチューニングを行うことで、Text-to-Imageモデルに新たな被写体を学習させる手法です。  特徴は、次のとおりです。 ・Dreamboothより高速 ・VRAM 8GBでも動作 ・学習データだけ抽出して他モデルとマージできる ・学習結果のサイズが小さい (Unet のみで3MB、Unet+Clipで6MB) ・UnetとCLIPの両方をファインチューニング可能。 2. ファ
		</p>
		<p class="rich-link-href">
		https://note.com/npaka/n/ndb287a48b682
		</p>
	</div>
</a></div>

Low-rank Adaptation : 数枚の画像と対応するテキストを元にファインチューニングすることで、Text-to-Image モデルに新たな被写体を学習させる手法とある。

うーんハードル高し。


