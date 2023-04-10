---
type: note
---

#ai

---
2023-03-28  09:37

# ai  RWKV がすごいらしい


<div class="rich-link-card-container"><a class="rich-link-card" href="https://note.com/shi3zblog/n/na991171b8fdd" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://assets.st-note.com/production/uploads/images/101199727/rectangle_large_type_2_0a65b23ab66fffde813395675bdf7c93.png?fit=bounds&quality=85&width=1280')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">完全フリーで3GBのVRAMでも超高速に動く14B大規模言語モデルRWKVを試す｜shi3z｜note</h1>
		<p class="rich-link-card-description">
		Transformerは分散できる代償として計算量が爆発的に多いという不利がある。  一度みんなが忘れていたリカレントニューラルネットワーク(RNN)もボケーっとしている場合ではなかった。  なんと、GPT3並の性能を持つ、しかも完全にオープンな大規模言語モデルが公開されていた。  そのなもRWKV(RuwaKuvと発音しろと書いてある。ルワクフ?) RWKVはRNNなのでGPUメモリをそれほど大量に必要としない。 3GBのVRAMでも動くという。  時間がない方はビデオをご覧ください それに何より完全にフリーである。Apacheライセンス2.0 これはまさに民主化。
		</p>
		<p class="rich-link-href">
		https://note.com/shi3zblog/n/na991171b8fdd
		</p>
	</div>
</a></div>



RNN だそうだ!! だから、小さいリソースでも動くらしいのだが、どうやるのだろうか。



<div class="rich-link-card-container"><a class="rich-link-card" href="https://github.com/BlinkDL/RWKV-LM" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://opengraph.githubassets.com/e3158d9de9685f00a3a2a0fa61b360665dc0e176b38d18b0d071ea9664695e27/BlinkDL/RWKV-LM')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">GitHub - BlinkDL/RWKV-LM: RWKV is an RNN with transformer-level LLM performance. It can be directly trained like a GPT (parallelizable). So it's combining the best of RNN and transformer - great performance, fast inference, saves VRAM, fast training, "infinite" ctx_len, and free sentence embedding.</h1>
		<p class="rich-link-card-description">
		RWKV is an RNN with transformer-level LLM performance. It can be directly trained like a GPT (parallelizable). So it&amp;#39;s combining the best of RNN and transformer - great performance, fast in...
		</p>
		<p class="rich-link-href">
		https://github.com/BlinkDL/RWKV-LM
		</p>
	</div>
</a></div>


やっぱり CPU だとおに鬼遅いらしい。

