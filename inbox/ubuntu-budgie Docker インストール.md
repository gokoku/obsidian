---
type: note
---

#ubuntu #docker 

---
2023-05-29  13:59

# ubuntu-budgie Docker インストール


<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.perplexity.ai/search/f95b1393-b754-470c-af1f-c4ebd0e632ef?s=u" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://ppl-ai-public.s3.amazonaws.com/previews/f95b1393-b754-470c-af1f-c4ebd0e632ef-preview-1685336305.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">on ubuntu 24 docker install</h1>
		<p class="rich-link-card-description">
		To install Docker on Ubuntu 24, you can follow the instructions provided by Docker's official documentation[1]. Here's a summary of the steps:  1. Update the apt package index and install packages to allow apt to use a repository over HTTPS:...
		</p>
		<p class="rich-link-href">
		https://www.perplexity.ai/search/f95b1393-b754-470c-af1f-c4ebd0e632ef?s=u
		</p>
	</div>
</a></div>

on ubuntu 24 docker install

```shell
$ sudo apt update
$ sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release
```

GPG key
```shell
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

stable repository
```shell
$ echo \ "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \ $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```shell
$ sudo apt update
$ sudo apt install docker-ce docker-ce-cli containerd.io
```

running the hello-world image
```shell
$ sudo docker run hello-world
```

## sudo 無しで


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/katoyu_try1/items/1bdaaad9f64af86bbfb7" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9dWJ1bnR1JUU3JTg5JTg4RG9ja2VyJUUzJTgyJTkyJUU2JUFGJThFJUU1JTlCJTlFc3VkbyVFMyU4MSVBQSVFMyU4MSU5NyVFMyU4MSVBNyVFNSVBRSU5RiVFOCVBMSU4QyVFMyU4MSU5NyVFMyU4MSU5RiVFMyU4MSU4NCZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9NTUxMWY4MzYyYmM1OTNmMTA3NDBiNDkwZWVmZjk3M2Y&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwa2F0b3l1X3RyeTEmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTUzYTEzYTYwZTEzZTVlNjZlY2IwM2M0NmE3NDgwY2Vh&blend-x=142&blend-y=491&blend-mode=normal&s=ea5a1d98e6fadf2fa8f897f840324d3a')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">ubuntu版Dockerを毎回sudoなしで実行したい - Qiita</h1>
		<p class="rich-link-card-description">
		はじめに ubuntu内にDockerをインストールしたけどコマンドを実行する前にsudoをつけないといけないのが面倒なのでどうにかしたい。 主に以下の記事を参考にしました。 dockerコマンドをsudoの付与無しに実行できるよう...
		</p>
		<p class="rich-link-href">
		https://qiita.com/katoyu_try1/items/1bdaaad9f64af86bbfb7
		</p>
	</div>
</a></div>

docker group にエントリーしているユーザー

```shell
$ getent group docker
docker:x:995:
```
いない。

administrator ユーザーを追加する。
```shell
$ sudo gpasswd -a administrator docker
```
確認
```shell
$ id administrator
......995(docker)
```

再起動して、確認。
```shell
$ docker ps
```

OK!!
