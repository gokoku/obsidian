---
type: note
---

#asdf #clojure

---
2023-04-04  10:01

# asdf Leiningen 2がinstall 出来なかった

```shell
$ asdf install lein 2.10.0
jqがないと言われる
```

jq って node のライブラリのあれ?

```shell
npm -g install jq
```

これが module のあれがないこれがないの繰り返し、最後は cli は winston@3.0.0 でremove されたよ、で詰む。

なんと、asdf にjq専用のパッケージがある!!
```shell
$ asdf plugin-add jq https://github.com/AZMCode/asdf-jq.git
$ asdf list all jq
$ asdf install jq 1.6
$ asdf global jq 1.6
```

これで、leiningen 2.10.0 が入った。
```shell
$ asdf install lein 2.10.0
$ asdf global lein 2.10.0

$ lein
```


Leiningen にも問題があって clojure-cli が手軽でいいらしい。

https://zenn.dev/uochan/articles/2022-05-26-build-edn


<div class="rich-link-card-container"><a class="rich-link-card" href="https://zenn.dev/uochan/articles/2022-05-26-build-edn" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://res.cloudinary.com/zenn/image/upload/s---h4w-S4I--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Clojure%2520CLI%2520%25E3%2582%2592%25E4%25BD%25BF%25E3%2581%25A3%25E3%2581%259F%25E3%2583%25A9%25E3%2582%25A4%25E3%2583%2596%25E3%2583%25A9%25E3%2583%25AA%25E3%2581%25AE%25E3%2583%2593%25E3%2583%25AB%25E3%2583%2589%25E3%2582%2584%25E3%2583%25AA%25E3%2583%25AA%25E3%2583%25BC%25E3%2582%25B9%25E3%2582%2592%25E6%25A5%25BD%25E3%2581%25AB%25E3%2581%2599%25E3%2582%258B%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:uochan%2Cx_203%2Cy_98/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2czT2MxdHNRZjVGd2pfMzNYb1VIODJNRnRtQTN4SUVtVTZvYmh4bEE9czI1MC1j%2Cr_max%2Cw_90%2Cx_87%2Cy_72/og-base.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Clojure CLI を使ったライブラリのビルドやリリースを楽にする</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://zenn.dev/uochan/articles/2022-05-26-build-edn
		</p>
	</div>
</a></div>

