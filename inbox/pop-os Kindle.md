---
type: note
---

#pop_os

---
2022-05-19  09:52

# pop-os Kindle

Kindle Cloud Reader では、
- 固定レイアウトの和書
- all of 洋書
だけが読める。

リフロー型の和書(文字主体の本)は読めない。

なので、



<div class="rich-link-card-container"><a class="rich-link-card" href="https://gihyo.jp/admin/serial/01/ubuntu-recipe/0433?page=1" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://gihyo.jp/assets/images/ICON/2008/112_ubuntuRecipe.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">第433回　Kindle UnlimitedをUbuntuで利用する</h1>
		<p class="rich-link-card-description">
		先週，Amazonの月額制読み放題サービスであるKindle Unlimitedが日本でもスタートしました。そこで今回はUbuntu上でKindle書籍を読む方法を解説します。ついでにKindle用の書籍データを作成する方法も紹介しましょう。
		</p>
		<p class="rich-link-href">
		https://gihyo.jp/admin/serial/01/ubuntu-recipe/0433?page=1
		</p>
	</div>
</a></div>





<div class="rich-link-card-container"><a class="rich-link-card" href="https://petit-noise.net/blog/use-kindle-for-pc-on-ubuntu-21-04/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://petit-noise.net/favicon.ico')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Ubuntu 21.04 で Kindle for PC を使う</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://petit-noise.net/blog/use-kindle-for-pc-on-ubuntu-21-04/
		</p>
	</div>
</a></div>

これ行ってみよう。

## Wine v6 install

```shell
$ sudo dpkg --add-architecture i386

$ wget -qO - https://dl.winehq.org/wine-builds/winehg.key | sudo apt-key add -

warning 出る
Warning: apt-key is deprecated. Manage keying files in trusted.gpg.d instead (see apt-key(8))
```

[[pop-os  Add-key で warning]]

やめた。

Shop で入れてみた。
Wine 6.0.3 だ

https://read.amazon.co.jp

で読めないやつを突くと
![[Pasted image 20220519131836.png]]

ここでPC&Mac を突くと exe ファイルがダウンロードされるので、これを wine で動かす。

```shell
# 事前にこのディレクトリが必要
$ mkdir -p ~/.wine/drive_c/users/$USER/AppData/Local/Amazon/Kindle

$ wine ./KindleForPC-installer-1.36.65107.exe

```

動いたが、これは登録出来ん。

![[Pasted image 20220519131531.png | 500]]


ここの記事でもこの現象が書いてあった。
<div class="rich-link-card-container"><a class="rich-link-card" href="https://ill-identified.hatenablog.com/entry/2021/07/06/203222" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://hatenablog-parts.com/embed?url=https%3A%2F%2Fill-identified.hatenablog.com%2Fentry%2F2021%2F07%2F06%2F203222')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">[日記] Ubuntu 20.04でWine v6を使いKindleを動かす - ill-identified diary</h1>
		<p class="rich-link-card-description">
		要点 Gecko がないと Kindle 上で Amazon のログインページがうまく表示されないらしい Wine v6.0 台でないと上記はサポートされないらしい 以下の投稿を既に見ている人にとっては新しい情報はほぼないので以降を読む価値はない. 近頃Wineで動かしていたKindle for PCがネットワークエラ…
		</p>
		<p class="rich-link-href">
		https://ill-identified.hatenablog.com/entry/2021/07/06/203222
		</p>
	</div>
</a></div>


winetricks を install してみる。

```shell
$ sudo apt install winetricks
winetricks
```

![[Pasted image 20220519140116.png]]

![[Pasted image 20220519140208.png]]

![[Pasted image 20220519140314.png]]

ダイアログは全部OKにした。

![[Pasted image 20220519140600.png]]

山ほどダイアログをOKにしたら

![[Pasted image 20220519141850.png]]

Kindle 立ち上げたら、ダメだった。

次、これ入れる。
![[Pasted image 20220519142122.png]]

いっぱいあって分からない。
xquartz を入れてみる。GUI っぽいから。
凄い時間がかかる。

おまけに失敗したっぽい。

### wine の window バージョン

```shell
$ winecfg
```

Windowsバージョンが　7 だったので 10 にしてみた。



結局ダメだった。






