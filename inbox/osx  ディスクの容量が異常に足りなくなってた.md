---
type: note
---

#osx #xcode

---
2022-05-20  22:18

# osx  ディスクの容量が異常に足りなくなってた

1T で /system/volume/data が ほとんど食って、xcode がインストール出来なくなってた

リンゴマーク-->このMac について-->ストレージ

![[Pasted image 20220520223806.png | 500]]

ストレージ残り　30G という異常なことに。

```shell
$ sudo du -g -x -d 5 / | awk '$1 >= 5{print}'
```

これで 5階層までで 5G以上のディレクトリを表示する。

見てると、/Library/InstallerSandboxes/.PKInstallSandboxManager/... が一杯あって、物凄い容量を食ってる。

これは xcode のインストールの時に作られて、インストール終わると消えるとのこと。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://zenn.dev/tomoyukik/articles/19f9a3cf3c35c0" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://res.cloudinary.com/zenn/image/upload/s--zTNNKyf_--/co_rgb:222%2Cg_south_west%2Cl_text:notosansjp-medium.otf_37_bold:tomoyukik%2Cx_203%2Cy_98/c_fit%2Cco_rgb:222%2Cg_north_west%2Cl_text:notosansjp-medium.otf_70_bold:Mac%25E3%2581%25AE%25E3%2582%25B9%25E3%2583%2588%25E3%2583%25AC%25E3%2583%25BC%25E3%2582%25B8%25E3%2582%2592%25E3%2580%258C%25E3%2581%259D%25E3%2581%25AE%25E4%25BB%2596%25E3%2580%258D%25E3%2581%258C%25E5%259C%25A7%25E8%25BF%25AB%25E5%2595%258F%25E9%25A1%258C%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9yZXMuY2xvdWRpbmFyeS5jb20vemVubi9pbWFnZS9mZXRjaC9zLS1jbkFQS0RoWS0tL2NfbGltaXQlMkNmX2F1dG8lMkNmbF9wcm9ncmVzc2l2ZSUyQ3FfYXV0byUyQ3dfNzAvaHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzI5YTgxYjVhODguanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_72/v1627274783/default/og-base_z4sxah.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Macのストレージを「その他」が圧迫問題</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://zenn.dev/tomoyukik/articles/19f9a3cf3c35c0
		</p>
	</div>
</a></div>

xcode インストール途中で容量が足りなくなって残ってしまったのか。

とりあえず削除してみる。

```shell
$ sudo rm –rf /Library/IndtallerSandoxes/.PKInstallSandboxManager
```

ものすごく時間がかかる。


## 会社のマックのクリーンインストール

今こうなった。
![[Pasted image 20220526184756.png]]

いいことだ。
起動もめちゃ速くなったし。
