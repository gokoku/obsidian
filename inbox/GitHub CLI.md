---
type: note
---

	#github 

---
2022-05-21  19:30

# GitHub CLI

<div class="rich-link-card-container"><a class="rich-link-card" href="https://github.com/github/homebrew-gh" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://opengraph.githubassets.com/c12b6af38fbd195e80b49e55cd4c2be17d61bba369a2837dd0315801946394c4/github/homebrew-gh')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">GitHub - github/homebrew-gh: Homebrew tap for the GitHub CLI</h1>
		<p class="rich-link-card-description">
		Homebrew tap for the GitHub CLI. Contribute to github/homebrew-gh development by creating an account on GitHub.
		</p>
		<p class="rich-link-href">
		https://github.com/github/homebrew-gh
		</p>
	</div>
</a></div>



```shell
$ brew install gh


$ brew upgrade gh
```

![[Pasted image 20220521193245.png | 400]]

二段階認証を設定しなくては使えない。

![[github  2段階認証にする]]

```shell
$ gh auth login

>GitHub.com
>SSH
>Generate a new SSH key to add to your GitHub account n
>brawsar 承認する
>
```

あれ？うまくいかない。
iPhone に来ないな。

## authentication token にしてみる

![[Pasted image 20220521231150.png | 600]]

![[Pasted image 20220521231822.png | 600]]


Personal access tokens で許可するものが repo と read:org を ON にして Generate した。

```shell
Paste your authentication token:** ************************************
- gh config set -h github.com git_protocol ssh
✓ Configured git protocol
✓ Logged in as **gokoku**
```

出来た！

![[Pasted image 20220521232308.png | 400]]

GitHub CLI やってみよう。

```shell
$ gh repo clone gokoku/zsh

普通に clone 出来た。
```



