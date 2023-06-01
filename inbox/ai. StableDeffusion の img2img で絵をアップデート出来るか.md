---
type: note
---

#ai

---
2022-08-26  10:30

# ai. StableDeffusion の img2img で絵をアップデート出来るか

http://cedro3.com/ai/image2image/

![[Pasted image 20220826103223.png]]

```python
!pip install transformers gradio scipy ftfy "ipywidgets>=7,<8" datasets
!git clone https://github.com/huggingface/diffusers.git
!pip install git+https://github.com/huggingface/diffusers.git
%cd diffusers
```

cd でディスクが使えるのか。

![[Pasted image 20220826103339.png]]

```python
from huggingface_hub import notebook_login
notebook_login()
```


![[Pasted image 20220826104917.png]]


```python
#@title **本体プログラム**

import gradio as gr
import torch
from torch import autocast
from diffusers import StableDiffusionPipeline, LMSDiscreteScheduler
import requests
from PIL import Image
from io import BytesIO
from IPython.display import clear_output ###
from examples.inference.image_to_image import StableDiffusionImg2ImgPipeline, preprocess

lms = LMSDiscreteScheduler(
	beta_start=0.00085,
	beta_end=0.012,
	beta_schedule="scaled_linear"
)

pipe = StableDiffusionPipeline.from_pretrained(
	"CompVis/stable-diffusion-v1-4",
	scheduler=lms,
	revision="fp16",
	use_auth_token=True
).to("cuda")

pipeimg = StableDiffusionImg2ImgPipeline.from_pretrained(
	"CompVis/stable-diffusion-v1-4",
	revision="fp16",
	torch_dtype=torch.float16,
	use_auth_token=True
).to("cuda")

block = gr.Blocks(css=".container { max-width: 800px; margin: auto; }")
num_samples = 2

def infer(prompt, init_image, strength):
	if init_image != None:
		init_image = init_image.resize((512, 512))
		init_image = preprocess(init_image)
		with autocast("cuda"):
			images = pipeimg([prompt] * num_samples, init_image=init_image, strength=strength, guidance_scale=7.5)["sample"]

	else:
		with autocast("cuda"):
			images = pipe([prompt] * num_samples, guidance_scale=7.5)["sample"]

	return images

with block as demo:
	gr.Markdown("<h1><center>Stable Diffusion</center></h1>")
	gr.Markdown(
		"Stable Diffusion is an AI model that generates images from any prompt you give!"
	)
	with gr.Group():
		with gr.Box():
			with gr.Row().style(mobile_collapse=False, equal_height=True):

				text = gr.Textbox(
					label="Enter your prompt", show_label=False, max_lines=1
				).style(
					border=(True, False, True, True),
					rounded=(True, False, False, True),
					container=False,
				)
				btn = gr.Button("Run").style(
					margin=False,
					rounded=(False, True, True, False),
				)
		strength_slider = gr.Slider(
			label="Strength",
			maximum = 1,
			value = 0.75
		)
		image = gr.Image(
			label="Intial Image",
			type="pil"
		)

		gallery = gr.Gallery(label="Generated images", show_label=False).style(
grid=[2], height="auto"
		)
		text.submit(infer, inputs=[text,image,strength_slider], outputs=gallery)
		btn.click(infer, inputs=[text,image,strength_slider], outputs=gallery)

	gr.Markdown(

		"""___
	<p style='text-align: center'>
	Created by CompVis and Stability AI
	<br/>
	</p>"""
	)

clear_output() ###
demo.launch(debug=True)
```

![[Pasted image 20220826105651.png | 500]]

すごい、こんな GUI作っちゃえるんだこのお方!!

![[sss.png |200]]![[download-1.png | 200]]![[download 1.png | 200]]

まあまあ、少し不気味さも愛嬌かな?

自分のクセを学習してくれる仕組みが欲しいな。

オレの相棒AIアシスタント計画!!!

