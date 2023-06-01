---
type: note
---

#clojure 

---
2023-04-19  23:09

# clojure を今 mac で動かす


<div class="rich-link-card-container"><a class="rich-link-card" href="https://memoribuka-lab.com/?p=3569" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://memoribuka-lab.com/wp-content/uploads/2020/08/Twitter_eye_catch.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Mac編 VS Code で Clojure を動かすために必要な準備 | ibukish Lab+</h1>
		<p class="rich-link-card-description">
		ClojureをVSCodeで触ってみたいけど何をしたらいいのかわからないってお困りですか？VS CodeでClojureを使ってREPL駆動開発をするためには他の言語に比べると環境構築に少し手間がかかります。自分も環境構築には少し苦労したので備忘も含めて記事にしました。環境構築でお困りの方は是非参考にしてください。
		</p>
		<p class="rich-link-href">
		https://memoribuka-lab.com/?p=3569
		</p>
	</div>
</a></div>


asdf で入れる。
clojure と leiningen どちらも必要なのか。

```shell
$ asdf plugin add clojure https://github.com/asdf-community/asdf-clojure.git

$ asdf install clojure latest
$ asdf global clojure latest
$ clojure --version

Clojure CLI version 1.11.1.1273
```

```shell
$ asdf plugin-add lein https://github.com/miorimmax/asdf-lein.git

$ asdf install lein latest
$ asdf global lein latest
$ lein --version

Leiningen 2.9.9 on Java 18.0.2 OpenJDK 64-Bit Server VM
```

JAVAがないとあかん。

```shell
$ asdf plugin add java https://github.com/halcyon/asdf-java.git
$ asdf install java openjdk-20.0.1
$ asdf global java openjdk-20.0.1
```



vscode の extention. Calva

![[Pasted image 20230419231903.png]]

## vscode で使ってみる

```shell
$ lein new sample_clojure_app
$ cd samploe_clojure_app
$ code .
```

project.clj ファイルがあるところで開くらしい。

左下の REPL をクリックする。

![[Pasted image 20230419232402.png]]
Start your project with & REPL_and connect (a.k.a. Jack-in)　を選択

![[Pasted image 20230419232552.png]]

Leiningen を選択

![[Pasted image 20230419232745.png]]

REPL が開いた。


```clojure
; Use `alt+enter` to evaluate
```
alt + enter で 評価。

### ファイルのコードを評価する

src/core.clj を開いてみる。

![[Pasted image 20230419233310.png]]

式の後ろで ctrl + enter


