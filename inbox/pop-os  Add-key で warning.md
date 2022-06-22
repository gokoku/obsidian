---
type: note
---

#command #pop_os 

---
2022-05-19  10:30

# pop-os  Add-key で warning

```shell

$ wget -qO - https://dl.winehq.org/wine-builds/winehq.key | sudo apt-key add -

Warning: apt-key is deprecated. Manage keying files in trusted.gpg.d instead (see apt-key(8))
```

apt-key(8) は非推奨になったと。

https://www.clear-code.com/blog/2021/5/5.html


<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.clear-code.com/blog/2021/5/5.html" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.clear-code.com/images/icon.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">非推奨となったapt-keyの代わりにsigned-byとgnupgを使う方法 - 2021-05-05 - ククログ</h1>
		<p class="rich-link-card-description">
		<p><code>apt-key(8)</code>はapt 2.1.8より非推奨となりました。</p> <p>Ubuntu 20.10 (Groovy)やそろそろリリースの時期が近づいてきたDebian 11(Bullseye)からは<code>apt-key</code>を利用すると警告されます。</p> <div class="language-text highlighter-rouge"><div class="highlight"><pre class="highlight"><code>Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)). </code></pre></div></div> <p>今回は<code>apt-key</code>を使うのをやめて、代替手段へと(パッケージ提供側が)移行する方法について紹介します。</p>
		</p>
		<p class="rich-link-href">
		https://www.clear-code.com/blog/2021/5/5.html
		</p>
	</div>
</a>

apt-key は公開鍵のインポート --> /etc/apt/trusted.gpg

信頼する鍵として追加は不用心だとのこと。

## apt-key から移行する

gnupg を使ってキーリングに変換して、対象リポジトリと紐付けるのがおすすめらしい。

```shell
#変換
$ gpg --no-default-keyring --keyiring ./archive-keyring.gpg --import archive.key

#確認
$ file archive-keyring.gpg
```
なぜか ~ファイルがペアで出来てる。

##### これを対象リポジトリと紐付ける。
`/usr/share/keyings/` 以下にサードパーティの鍵を置くのがいいらしい。

```shell
$ sudo mv ./archive-keyring.gpg /usr/share/keyrings/
```

/etc/apt/sources.list.d/archive.list

```
deb [signed-by=/usr/share/keyrings/archive-keyring.gpg] https://packages.example.com/debian stable main
```

とするらしい。







