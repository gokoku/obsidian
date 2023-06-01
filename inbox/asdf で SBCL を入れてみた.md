---
type: note
---

#lisp #asdf

---
2022-06-07  20:37

# asdf で SBCL を入れてみた



```shell
$ npm install -g jq

$ asdf plugin add sbcl
$ asdf list all sbcl
No compatible versions available (sbcl )

??
でも、これで入ってしまう

$ asdf install sbcl 2.2.0

$ asdf global sbcl 2.2.0
$ sbcl
* (+ 1 1)
* 2
```

立派に動く。

<div class="rich-link-card-container"><a class="rich-link-card" href="https://github.com/smashedtoatoms/asdf-sbcl" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://opengraph.githubassets.com/86647acb4f13bbc1e6abed2b25d4e2a64db11a8191549bfd4ac40c8f79086039/smashedtoatoms/asdf-sbcl')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">GitHub - smashedtoatoms/asdf-sbcl: asdf plugin for Steel Bank Common Lisp</h1>
		<p class="rich-link-card-description">
		asdf plugin for Steel Bank Common Lisp. Contribute to smashedtoatoms/asdf-sbcl development by creating an account on GitHub.
		</p>
		<p class="rich-link-href">
		https://github.com/smashedtoatoms/asdf-sbcl
		</p>
	</div>
</a></div>



