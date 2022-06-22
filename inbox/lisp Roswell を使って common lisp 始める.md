---
type: note
---

#lisp #pop_os 

---
2022-06-07  20:36

# lisp Roswell を使って common lisp 始める


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/t-sin/items/054c2ff315ec3b9d3bdc" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9JUUzJTgxJTg0JUUzJTgxJUJFJUUzJTgxJThCJUUzJTgyJTg5JUU1JUE3JThCJUUzJTgyJTgxJUUzJTgyJThCQ29tbW9uJTIwTGlzcCZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9ZmU0ODhiMzY3NTQ5MjNlYTQ5ZTNjYTVmNDMzYWZmZGQ&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwdC1zaW4mdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPWUxMTgyZDQ2NTVmNzQwOTk4MGE2Y2Y3NGJhZGQ1MTEx&blend-x=142&blend-y=491&blend-mode=normal&s=1d13db8197e7473a16f174896947ec6a')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">いまから始めるCommon Lisp - Qiita</h1>
		<p class="rich-link-card-description">
		この記事はLisp Advent Calendar 2017の二日目の記事です。 はじめに  この記事は、Common Lispという初めての人には初めましてな言語の入門記事です。  この世には、Common Lispというとって...
		</p>
		<p class="rich-link-href">
		https://qiita.com/t-sin/items/054c2ff315ec3b9d3bdc
		</p>
	</div>
</a></div>



```shell
$ brew install roswell

$ ros setup
$ ros run
* (+ 1 2)
* 3
```

SBCL が `*` がプロンプトとのこと。

error が出たら abort と打つとのこと。

### editor lem
快適な環境を使いたい。emacs だろうか。
lem というものがある。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://masatoi.hateblo.jp/entry/20161107/1478531952" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://hatenablog-parts.com/embed?url=https%3A%2F%2Fmasatoi.hateblo.jp%2Fentry%2F20161107%2F1478531952')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">lem: Common Lispで書かれたEmacsライクなエディタlemを使ってみた - masatoi’s blog</h1>
		<p class="rich-link-card-description">
		lem: https://github.com/cxxxr/lem lemはCommon Lispで書かれたEmacsライクなエディタで、拡張もCommon Lispで書ける。cl-charmsというncursesのCFFIラッパーを使っている。 特に何もしてなくても起動が速いが、lemをロードした状態で処理系のコアイ…
		</p>
		<p class="rich-link-href">
		https://masatoi.hateblo.jp/entry/20161107/1478531952
		</p>
	</div>
</a></div>

この人の使い方は高度だな。

```shell
$ ros install cxxxr/lem
```

`export PATH=$HOME/.roswell/bin:$PATH` 

```shell
$ lem

アップデート
$ ros update lem
```


<div class="rich-link-card-container"><a class="rich-link-card" href="https://github.com/lem-project/lem" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://opengraph.githubassets.com/06ae5b46b7f1098aa92b4adbf7de23d937617fb6f068dbbb78069a6f6ea7c4fc/lem-project/lem')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">GitHub - lem-project/lem: Common Lisp editor/IDE with high expansibility</h1>
		<p class="rich-link-card-description">
		Common Lisp editor/IDE with high expansibility. Contribute to lem-project/lem development by creating an account on GitHub.
		</p>
		<p class="rich-link-href">
		https://github.com/lem-project/lem
		</p>
	</div>
</a></div>
config は ~/.lem/init.lisp に書くようだ。

https://github.com/fukamachi/.lem

ここにある.lem を参考にしてみた(vi 以外)


<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.youtube.com/watch?v=YkSJ3p7Z9H0" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.youtube.com/embed/YkSJ3p7Z9H0?feature=oembed')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">lem : the editor for Common Lispers</h1>
		<p class="rich-link-card-description">
		a simple demo of lem
		</p>
		<p class="rich-link-href">
		https://www.youtube.com/watch?v=YkSJ3p7Z9H0
		</p>
	</div>
</a></div>

こんな風にやるにはどうやるの?

![[スクリーンショット 2022-06-07 22.05.35.png | 500]]

iTerm で Esc キーを Option キーに持ってきたら、CUI Emacs が Option で Meta キーになってくれて便利。

Lem も iTerm 上で動くので、Option が Meta key になってくれた。



