---
type: note
---

#ai

---
2022-12-07  13:14

# ai ChatGPT を使ってみる


<div class="rich-link-card-container"><a class="rich-link-card" href="https://beta.openai.com/overview" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://openaiapi-site.azureedge.net/public-assets/d/1c781c83ea/favicon.svg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">OpenAI API</h1>
		<p class="rich-link-card-description">
		An API for accessing new AI models developed by OpenAI
		</p>
		<p class="rich-link-href">
		https://beta.openai.com/overview
		</p>
	</div>
</a></div>

登録した。

ChatGPT ってこれ?
https://chat.openai.com/chat

![[Pasted image 20221207131631.png|500]]

# python で使う

Libraries で書き方があるが、なんのことか。

https://beta.openai.com/docs/libraries/python-bindings

```shell
$ asdf plugin add python
$ asdf list all python
$ asdf install python 3.11.0
$ asdf global python 3.11.0

Mac のやつと被ってこんがらがるのでリネームした
$ sudo mv /usr/local/bin/pip3 /usr/local/bin/pip3_org
$ sudo mv /usr/local/bin/pytyon3 /usr/local/bin/python3_org

$ pip install openai
```


なんかよくわからんけど、これで会話できるっぽい。

元は OpenAI のサンプルで chat をコピペした。

```shell
$ pip install openai
$ pip install python-dotenv
```

.env にAPI_KEYを書く。
```shell
OPENAI_API_KEY="....."
```

```python
import os
import openai
from dotenv import load_dotenv
load_dotenv()

openai.api_key = os.getenv("OPENAI_API_KEY")


while True:
  my_prompt = input("You : ")

  response = openai.Completion.create(
    model="text-davinci-003",
    prompt=my_prompt,
    temperature=0.9,
    max_tokens=150,
    top_p=1,
    frequency_penalty=0.0,
    presence_penalty=0.6,
    stop=[" Human:", " AI:"]
  )

  print("OpenAI : {}\n".format(response['choices'][0]['text']))
```


openai を色々切り替えてらしくするのかな。

因みに playground では、なんでもござれだ。

https://beta.openai.com/playground

![[Pasted image 20221207152027.png]]

python のこと尋ねながらこのスクリプト書いた。

