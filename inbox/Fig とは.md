---
type: note
---

#command

---
2022-04-25  17:53

# Fig とは?


<div class="rich-link-card-container"><a class="rich-link-card" href="https://zenn.dev/huuya/articles/1d0e56a4c7c55c" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://res.cloudinary.com/zenn/image/upload/s--xkCrngXW--/co_rgb:222%2Cg_south_west%2Cl_text:notosansjp-medium.otf_37_bold:huuya%2Cx_203%2Cy_98/c_fit%2Cco_rgb:222%2Cg_north_west%2Cl_text:notosansjp-medium.otf_65_bold:%25E3%2582%25A2%25E3%2583%25BC%25E3%2583%25AA%25E3%2583%25BC%25E3%2582%25A2%25E3%2582%25AF%25E3%2582%25BB%25E3%2582%25B9%25E3%2581%25AE%25E7%2594%25B3%25E3%2581%2597%25E8%25BE%25BC%25E3%2581%25BF%25E3%2582%2592%25E3%2581%2597%25E3%2581%25A6%25E3%2581%2584%25E3%2581%259FFig%25E3%2581%258C%25E5%2588%25A9%25E7%2594%25A8%25E3%2581%25A7%25E3%2581%258D%25E3%2582%258B%25E3%2582%2588%25E3%2581%2586%25E3%2581%25AB%25E3%2581%25AA%25E3%2581%25A3%25E3%2581%259F%25E3%2581%25AE%25E3%2581%25A7%25E6%2597%25A9%25E9%2580%259F%25E8%25A7%25A6%25E3%2581%25A3%25E3%2581%25A6%25E3%2581%25BF%25E3%2581%259F%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9yZXMuY2xvdWRpbmFyeS5jb20vemVubi9pbWFnZS9mZXRjaC9zLS1FeERSOXBkbi0tL2NfbGltaXQlMkNmX2F1dG8lMkNmbF9wcm9ncmVzc2l2ZSUyQ3FfYXV0byUyQ3dfNzAvaHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2YzZDJmYTZjYjMuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_72/v1627274783/default/og-base_z4sxah.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">アーリーアクセスの申し込みをしていたFigが利用できるようになったので早速触ってみた</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://zenn.dev/huuya/articles/1d0e56a4c7c55c
		</p>
	</div>
</a></div>

常駐プロセスで、ターミナルからエディタまでサジェストしてくれるらしい。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://fig.io/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://fig.io/images/screenshots/git-zoomed.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Fig</h1>
		<p class="rich-link-card-description">
		Autocomplete for your terminal.
		</p>
		<p class="rich-link-href">
		https://fig.io/
		</p>
	</div>
</a></div>

Download for free で dmg --> /Applications

![[Pasted image 20220425175625.png]]

![[Pasted image 20220425175725.png | 400]]

![[Pasted image 20220425175757.png | 400]]

![[Pasted image 20220425175847.png]]
email 入れた。メールで認証コード入れた。

![[Pasted image 20220425180044.png | 300]]
どこで私たちのことを知りましたか? friend で Send した。

How usage

どうも、申し込みだけ。

---

# 動くようになった

どうも.zshrc の位置が変だったことが原因だった。
苦し紛れに ln 張った。

```shell
$ ln -s ~/zsh/.zshrc .zshrc
```

ターミナル再起動してチェック。

```shell
$ fig doctor
OK
```

dotfile のエディタは使えないけど、いいや。

![[Pasted image 20220510095556.png]]

こんな感じでかなり可愛い。



