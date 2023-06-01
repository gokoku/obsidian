---
type: note
---

#warp

---
2022-12-14  13:23

# warp  次世代ターミナル?!

```shell
$ brew install warp
```


<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.warp.dev/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://storage.googleapis.com/website-image-preview/www_warp_dev.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Warp: The terminal for the 21st century</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://www.warp.dev/
		</p>
	</div>
</a></div>
モダンなんだ。
Rust 製とのこと。Fast!!



<div class="rich-link-card-container"><a class="rich-link-card" href="https://zenn.dev/toono_f/articles/03cc961bfd64c1" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://res.cloudinary.com/zenn/image/upload/s--wCkdrKXo--/co_rgb:222%2Cg_south_west%2Cl_text:notosansjp-medium.otf_37_bold:%25E3%2581%258A%25E3%2581%25A8%25E3%2581%25AE%2Cx_203%2Cy_98/c_fit%2Cco_rgb:222%2Cg_north_west%2Cl_text:notosansjp-medium.otf_65_bold:iTerm%25E3%2582%2592%25E3%2582%2584%25E3%2582%2581%25E3%2581%25A6Warp%25E3%2581%25A8%25E3%2581%2584%25E3%2581%2586%25E3%2583%25A2%25E3%2583%2580%25E3%2583%25B3%25E3%2582%25BF%25E3%2583%25BC%25E3%2583%259F%25E3%2583%258A%25E3%2583%25AB%25E3%2582%25A2%25E3%2583%2597%25E3%2583%25AA%25E3%2582%2592%25E5%25B0%258E%25E5%2585%25A5%25E3%2581%2597%25E3%2581%259F%25E3%2582%2589%25E3%2580%2581%25E7%2594%259F%25E7%2594%25A3%25E6%2580%25A7%25E3%2581%258C%25E3%2582%2584%25E3%2581%25AF%25E3%2582%258A%25E7%2588%2586%25E4%25B8%258A%25E3%2581%258C%25E3%2582%258A%25E3%2581%2597%25E3%2581%259F%25E4%25BB%25B6%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9yZXMuY2xvdWRpbmFyeS5jb20vemVubi9pbWFnZS9mZXRjaC9zLS05cm96SHJaWS0tL2NfbGltaXQlMkNmX2F1dG8lMkNmbF9wcm9ncmVzc2l2ZSUyQ3FfYXV0byUyQ3dfNzAvaHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzAzNDEyM2FhZDkuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_72/v1627274783/default/og-base_z4sxah.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">iTermをやめてWarpというモダンターミナルアプリを導入したら、生産性がやはり爆上がりした件</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://zenn.dev/toono_f/articles/03cc961bfd64c1
		</p>
	</div>
</a></div>


terminal に iTerrm2 を使ってるのは、Theme が一杯あって可愛いからっていうのが実は大きいかも。
もはやシーラカンス的にほとんど変わらない状態。

モダンなターミナルの体験とはどんなだ?!!

![[Pasted image 20221214133845.png | 300]]

SIgn up がある。
GitHub アカウントで入る。


![[Pasted image 20221214134451.png]]

まず、zshrc が失敗してる。
fzf のgradeup でパスが変わっただけだった。

Pricing がある。サーバはここでもってるんだろうか。

なんの設定もしてないのに完璧な感じになってる。
一番便利だった fzf の履歴インクリメントサーチがデフォルトで使える状態だ。
しかも、ctrl + R と同じショートカットで動かせる。

つまり、このまま移行しても全然ありってことだ。


![[Pasted image 20221214142402.png]]

やばい。可愛くなってきた。

Tab を開くと cd が受け継がれて何気に便利。

これを使うことにする。




emacs で meta key を optionキーにしたい。

![[Pasted image 20221221102043.png]]


