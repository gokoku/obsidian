 #command 

---
[[2022-01-27]]  10:53

# command  Fig というものを使ってみるが失敗


<div class="rich-link-card-container"><a class="rich-link-card" href="https://fig.io/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://fig.io/images/screenshots/git-open-graph.png')">
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

CLI を監視しているプロセスとのこと。


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

入れてみる。

```shell
$ brew install --cask fig
```

![[Pasted image 20220127105730.png]]

ダブルクリック。

![[Pasted image 20220127105812.png]]

![[Pasted image 20220127105939.png]]

fig settings コマンド。

![[Pasted image 20220127110120.png]]

メアド入れんと進めん。
とにかく終わった。

なんか失敗してる。
メニューから Restart したら出た。

![[Pasted image 20220127110831.png]]
```shell
$ fig doctor
❌ Fig not in PATH
❌ Figterm socket does not exist at /tmp/figterm-w1t0p0:9129E191-F936-4E79-9B82-2ED0BC20465B.socket
❌ Could not get iTerm version

❌ Doctor found errors. Please fix them and try again.

If you are not sure how to fix it, please open an issue with fig issue to let us know!
Or, email us at hello@fig.io!
```

めっちゃバツ。

iTerm2 はサポートしてないみたい。iTerm だけのようだ。

残念。
