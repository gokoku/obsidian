---
type: note
---

#emacs #clojure 

---
2023-04-04  10:37

# emacs Clojure 再入門(何度目?)

M-x list-package で clojure-mode を install する
![[Pasted image 20230404103808.png]]


これだけでは動かなかった。

cider
cider-hydra

を入れたら、M-x cider-mode が現れた。
cider-jack-in もある。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/laughingmanbtc/items/31ed4fd332cfb5aa0155?utm_source=stock_summary_mail&utm_medium=email&utm_term=hozyo&utm_content=Clojure%E5%85%A5%E9%96%80%E3%83%A1%E3%83%A2%20(Python%20&%20Common%20Lisp%E7%B5%8C%E9%A8%93%E8%80%85)&utm_campaign=stock_summary_mail_2023-04-01" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9Q2xvanVyZSVFNSU4NSVBNSVFOSU5NiU4MCVFMyU4MyVBMSVFMyU4MyVBMiUyMCUyOFB5dGhvbiUyMCUyNiUyMENvbW1vbiUyMExpc3AlRTclQjUlOEMlRTklQTglOTMlRTglODAlODUlMjkmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT01NiZ0eHQtY2xpcD1lbGxpcHNpcyZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPWJlMzU0NDc2Y2VmOTY3NjY3NzBiYmYwZDUyZDNlM2Zl&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwbGF1Z2hpbmdtYW5idGMmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTIxMWM0MDYyMWMwYTRiNzRkMzA2NzdmY2ZlOWI2MDRh&blend-x=142&blend-y=491&blend-mode=normal&s=e27cb7e6a036b77e41650546e39064e9')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Clojure入門メモ (Python & Common Lisp経験者) - Qiita</h1>
		<p class="rich-link-card-description">
		Clojrueに入門したのでメモ。私自身は、PythonとCommon Lispに触れたことがあります。  気軽に走らせるならclojure-cli Clojureを動かそうと思って調べると、Leiningen (lein)、boot...
		</p>
		<p class="rich-link-href">
		https://qiita.com/laughingmanbtc/items/31ed4fd332cfb5aa0155?utm_source=stock_summary_mail&utm_medium=email&utm_term=hozyo&utm_content=Clojure%E5%85%A5%E9%96%80%E3%83%A1%E3%83%A2%20(Python%20&%20Common%20Lisp%E7%B5%8C%E9%A8%93%E8%80%85)&utm_campaign=stock_summary_mail_2023-04-01
		</p>
	</div>
</a></div>



```lisp
(leaf cider-mode
	  :doc "major mode for editing clojure"
	  :tag "builtin"
	  :mode-hook
	  (add-hook 'clojure-mode-hook #'cider-mode)
	  (add-hook 'clojure-mode-hook #'paredit-mode)
	  )
```

leaf が今一よくわかってないのだが。

# これでちょっとやってみる

sample.clj
```clojure
(def greet [name]
  (str "Hello, " name))
```

ここで M-x cider-jack-in する。

![[Pasted image 20230404120223.png]]
式を C-x C-e で評価すると

なんと Repl の方で読み込まれて使える!!

![[Pasted image 20230404120311.png]]



