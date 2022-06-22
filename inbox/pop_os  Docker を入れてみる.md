---
type: note
---

#pop_os #docker

---
2022-05-08  21:36

# pop_os  Docker を入れてみる


<div class="rich-link-card-container"><a class="rich-link-card" href="https://docs.docker.com/engine/install/ubuntu/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://docs.docker.com/images/docs@2x.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Install Docker Engine on Ubuntu</h1>
		<p class="rich-link-card-description">
		Instructions for installing Docker Engine on Ubuntu
		</p>
		<p class="rich-link-href">
		https://docs.docker.com/engine/install/ubuntu/
		</p>
	</div>
</a></div>



```shell
$ sudo apt update
$ sudo apt install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release


$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg


$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```shell
$ sudo apt update
$ sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin

$ sudo docker run hello-world

Hello from Docker!
```

やった!! 
これ chromebook !!

### sudo 無しで実行させるには

https://qiita.com/ITF_katoyu/items/1bdaaad9f64af86bbfb7
<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/ITF_katoyu/items/1bdaaad9f64af86bbfb7" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9dWJ1bnR1JUU3JTg5JTg4RG9ja2VyJUUzJTgyJTkyJUU2JUFGJThFJUU1JTlCJTlFc3VkbyVFMyU4MSVBQSVFMyU4MSU5NyVFMyU4MSVBNyVFNSVBRSU5RiVFOCVBMSU4QyVFMyU4MSU5NyVFMyU4MSU5RiVFMyU4MSU4NCZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9NTUxMWY4MzYyYmM1OTNmMTA3NDBiNDkwZWVmZjk3M2Y&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwSVRGX2thdG95dSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9NzQ1MTU4NDYwYjI0MTE0NzRjMjVkM2MyNWQ3NzRlNjE&blend-x=142&blend-y=491&blend-mode=normal&s=b29bcc4e8bbb3e70b3d2ce2cdff550fb')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">ubuntu版Dockerを毎回sudoなしで実行したい - Qiita</h1>
		<p class="rich-link-card-description">
		#はじめに ubuntu内にDockerをインストールしたけどコマンドを実行する前にsudoをつけないといけないのが面倒なのでどうにかしたい。 こちらを参考にしました。 [dockerコマンドをsudoの付与無しに実行できるようにする...
		</p>
		<p class="rich-link-href">
		https://qiita.com/ITF_katoyu/items/1bdaaad9f64af86bbfb7
		</p>
	</div>
</a></div>


```shell
$ getent group docker

docker:x:999:
```

docker グループに誰も入ってない。

```shell
$ sudo gpasswd -a george docker

$ getent group docker

docker:x:999:george

$ id george

uid=1000(george) gid=1000(george) groups=1000(george),4(adm),27(sudo),121(lpadmin),999(docker)
```

docker グループに入った。

再起動。

```shell
$ docker ps -a

```

OK!!

# Docker-compose を入れる必要はない

```shell
$ docker compose up
```





