---
type: note
---

#ai

---
2023-03-22  14:03

# ローカルで動くAI

https://gigazine.net/news/20230314-dalai-llama/


<div class="rich-link-card-container"><a class="rich-link-card" href="https://cocktailpeanut.github.io/dalai/#/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://cocktailpeanut.github.io/dalai/preview.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Dalai</h1>
		<p class="rich-link-card-description">
		Dead simple way to run LLaMA on your computer
		</p>
		<p class="rich-link-href">
		https://cocktailpeanut.github.io/dalai/#/
		</p>
	</div>
</a></div>


GUI 付きで動かせるらしい。

```shell
$ npx dalai llama install 7B
$ npx dalai serve
```

7B は 4GB モデル

http://localhost:3000

![[Pasted image 20230322144855.png]]

文章の続きを作ってくれるらしい。

数秒かかる、cpu かなり使われる。

