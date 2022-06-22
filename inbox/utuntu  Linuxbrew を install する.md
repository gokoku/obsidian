---
type: note
---

#ubuntu

---
2022-04-30  21:28

# utuntu  Linuxbrew を install する


<div class="rich-link-card-container"><a class="rich-link-card" href="https://docs.brew.sh/Homebrew-on-Linux" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://brew.sh/assets/img/linuxbrew.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Homebrew on Linux</h1>
		<p class="rich-link-card-description">
		Documentation for the missing package manager for macOS.
		</p>
		<p class="rich-link-href">
		https://docs.brew.sh/Homebrew-on-Linux
		</p>
	</div>
</a></div>


```shell
$ sudo apt install build-essential procps curl file git
```


```shell
$ test -d ~/.linuxbrew && eval "$(~/.linuxbrew/bin/brew shellenv)"
$ test -d /home/linuxbrew/.linuxbrew && eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```


.zshrc に
```shell
eval $($(brew --prefix)bin/brew shellenv) ではなく、brew --prefix が展開したやつ ↓

eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)

```


```shell
$ brew install hello
$ 
```



