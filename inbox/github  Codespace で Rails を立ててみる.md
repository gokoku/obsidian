---
type: note
---

#github 

---
2023-01-12  10:20

# github  Codespace で Rails を立ててみる

![[スクリーンショット 2023-01-12 10.20.58.png]]


rails のテンプレートを使ってみよう。

template を使うをクリックするとここまで自動で立ち上がる。

![[Pasted image 20230112102347.png]]

ちょっとエディットしてコミットする。

![[Pasted image 20230112103144.png]]

Publish Branch でリポジトリを GitHub に作る。
![[スクリーンショット 2023-01-12 10.53.59.png]]

![[スクリーンショット 2023-01-12 10.56.23.png]]




![[Pasted image 20230112110154.png]]

これで永続化したということか。

Codespace のコンテナをデリートした。

Git リポジトリからコンテナを create してみよう。

![[Pasted image 20230112110321.png]]

自動起動する

![[Pasted image 20230112110527.png]]


永続化しているのがわかる。

![[Pasted image 20230112110504.png]]


## つまり??

ローカルマシンのリソースに関係なくアプリが開発出来るレベルということか。

![[Pasted image 20230112110854.png]]

![GitHub Codespaces のしくみを示す図](https://docs.github.com/assets/cb-89172/images/help/codespaces/codespaces-diagram.png)

コンテナを都度 Stop させないと加算されちゃうんだろうな。
アクティブの間だけコンピューティング使用量が加算される。

停止と開始でコントロールするとある。
https://docs.github.com/ja/codespaces/developing-in-codespaces/stopping-and-starting-a-codespace

CPU料金は実行中の codespace に対して発生する。
停止中はストレージコストのみが発生する。

無料枠だと
* 15GB/月
* コア時間 120 時間/月　つまり、2コアで 60 時間/月


<div class="rich-link-card-container"><a class="rich-link-card" href="https://zenn.dev/dzeyelid/articles/a30a98618c40bd#%E7%A8%BC%E5%83%8D%E6%99%82%E9%96%93%E3%81%AE%E7%AE%A1%E7%90%86" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://res.cloudinary.com/zenn/image/upload/s--KSQBEilB--/co_rgb:222%2Cg_south_west%2Cl_text:notosansjp-medium.otf_37_bold:Kazumi%2520IWANAGA%2520...%2Cx_203%2Cy_98/c_fit%2Cco_rgb:222%2Cg_north_west%2Cl_text:notosansjp-medium.otf_65_bold:Check%2521%2520GitHub%2520Codespaces%2520%25E3%2581%258C%25E3%2582%2588%25E3%2582%258A%25E4%25BD%25BF%25E3%2581%2584%25E3%2582%2584%25E3%2581%2599%25E3%2581%258F%25EF%25BC%2581%25E7%25A5%259D%25E3%2583%25BB%25E4%25B8%2580%25E8%2588%25AC%25E5%2585%25AC%25E9%2596%258B%2520%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2pJNnV1eUJzeFNIRGVsMTBxX3JhRHBFSXVSNHlkcGV3QlB3VWNhPXM4MC1j%2Cr_max%2Cw_90%2Cx_87%2Cy_72/v1627274783/default/og-base_z4sxah.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Check! GitHub Codespaces がより使いやすく！祝・一般公開🎉</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://zenn.dev/dzeyelid/articles/a30a98618c40bd#%E7%A8%BC%E5%83%8D%E6%99%82%E9%96%93%E3%81%AE%E7%AE%A1%E7%90%86
		</p>
	</div>
</a></div>

codespace インスタンスを停止で稼働時間の課金を止められる。
一定時間(デフォルト30日)停止されていると自動的に削除。

インスタンスの名前は変更出来る。

まあ、15G以内なら使っては stop 使っては stop でいいのかな。




