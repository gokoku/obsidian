#notion

---
2022-04-13  11:52

# notion  Notion 入門



- [x] Notionで社員一覧表を作ろう #todo ✅ 2022-04-14
<div class="rich-link-card-container"><a class="rich-link-card" href="https://news.mynavi.jp/techplus/article/howtonotion-7/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://news.mynavi.jp/techplus/article/howtonotion-7/ogp_images/ogp.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Notionで社員一覧表を作ろう - Notionがあなたのチームを強くする(7)</h1>
		<p class="rich-link-card-description">
		今話題のNotionというツールをご存知でしょうか。Wiki、ドキュメント管理、プロジェクト管理といったチームに必要な情報をすべて1つに集約できるツールです。Notionを使ってあなたのチームをさらに強くする方法を解説します。
		</p>
		<p class="rich-link-href">
		https://news.mynavi.jp/techplus/article/howtonotion-7/
		</p>
	</div>
</a></div>

不正確だけど、こんなイメージ。
ページが1個のデータベースの時と、カラム(最小のパーツをこう呼ぶことにする)で出来てる時とどちらかになる。

一度ページをデータベースに決めたらカラムは入らず、一個のデータベースになる。

それ以外は、カラムがいくらでも add 出来てしかもその中にテキスト、画像、ファイルなどのコンテンツを入れられるし、データベースも入れられる。

またページの中にページを入れ子にも出来る。

なのでとても柔軟性が高すぎて応用範囲広すぎ。
データベースをベースにした、GUIコンテンツ作成ツール?!
CUI データベースの機能をそのまま GUI に作り込んだ GUIか簡易リレーショナルデータベースといったものがその正体のようだ。

![[notion 2022-04-13 22.41.10.excalidraw]]



**関数を使える**

ページを別なデータベースにして、既存データベースにアサインしてフィルターをかまして特別ビューに出来る。


さらに、1ページの中に既存データベースをフィルタリングしたものを複数入れられる。
抽出データと見せ方を自由自在に組み合わせられる。


左ペインのプラスはページを作る。
ページの中にページを作るとページへのリンク、つまりサブページへのリンク。
これが full page database というやつ。

もし、カラム(コンテンツとなる最小パーツをこう言おう)で inline database とあるものを選択すると、中身のデータがカラムに現れる。

どちらも左ペインでサブページとして見える。

![[Pasted image 20220413221053.png | 200]]
これらは親ページの子なので、必ず親コンテンツの中にある。
だから親コンテンツのこのカラムを消すとサブページが削除されるので、
左ペインからも無くなるので注意。

カラムで database table を選んで、new database を選択すると inline database と同じになる。
既存のデータベースを選ぶと

![[Pasted image 20220413221952.png]]

この状態はビューを確立してないモーダルな状態。
因みに New empty view を選択すると、ビューをどれかに選択出来る。
![[Pasted image 20220413222457.png]]

確定する。

![[Pasted image 20220413222613.png]]

しかし、ここでもデータベースソースもビューも変更可能だ。

Filter で絞り込んで知りたい情報だけ見えるようにするのが良さげ。
Group である項目で分けられる。

![[Pasted image 20220413223323.png | 500]]

しかも、表示したいグループだけの調整可能。

![[Pasted image 20220413223510.png| 200]]

まるっきりGUI データベースじゃないか!!　てゆーか、そう言っている。





- [x] Notionでプロジェクト管理をしよう #todo ✅ 2022-04-14



<div class="rich-link-card-container"><a class="rich-link-card" href="https://news.mynavi.jp/techplus/article/howtonotion-9/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://news.mynavi.jp/techplus/article/howtonotion-9/ogp_images/ogp.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Notionでプロジェクト管理をしよう - Notionがあなたのチームを強くする(9)</h1>
		<p class="rich-link-card-description">
		今話題のNotionというツールをご存知でしょうか。Wiki、ドキュメント管理、プロジェクト管理といったチームに必要な情報をすべて1つに集約できるツールです。Notionを使ってあなたのチームをさらに強くする方法を解説します。
		</p>
		<p class="rich-link-href">
		https://news.mynavi.jp/techplus/article/howtonotion-9/
		</p>
	</div>
</a></div>


だいぶサンプルテンプレートが分かるようになってた!!

- [x] これを改造して自分用のものを作ってみるか #todo ✅ 2022-04-26

データの質?の解析してからのデータベースの設計ではないか。

