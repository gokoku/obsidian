---
type: note
---

#alma_linux

---
2022-06-07  09:33

# AlmaLinux すげー

CentOS から CentOS Stream に重心を移すとしたとき、CloudLinux は代替OS プロジェクトを立ち上げたという。
それが AlmaLinux という。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://almalinux.org/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://almalinux.org/static/images/hero.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">AlmaLinux OS - Forever-Free Enterprise-Grade Operating System</h1>
		<p class="rich-link-card-description">
		An Open Source, community owned and governed, forever-free enterprise Linux distribution.
		</p>
		<p class="rich-link-href">
		https://almalinux.org/
		</p>
	</div>
</a></div>


これ要チェック。

docker あるね

```shell
$ docker search almalinux



$ docker pull almalinux
```

コンテナ作ってみる。

```shell
$ docker run -it almalinux /bin/bash
> dnf install emacs
```

dnf になりました。python3 ベースとのこと。




