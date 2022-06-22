---
type: note
---

#command

---
2022-04-28  16:26

# ssh プロキシクライアントを brew の connect にしてみる

どうも、Sequel Ace で Corkscrew が引っ掛かってる。
Corkscrew でないプロキシクライアントを使ってみたい。

mac に入ってる netcat がある。
```shell
/usr/bin/nc
```

どうも貧弱な気がするので、色々探したら nmap で入る netcat があるらしい。
nmap は入れてる。
```shell
$ which ncat
/usr/local/bin/ncat
```

結局上手くいかなかった。Mac だからか??

### connect を使ったら上手く行った!

<div class="rich-link-card-container"><a class="rich-link-card" href="https://blog.n-z.jp/blog/2018-08-12-ssh-over-proxy.html" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://blog.n-z.jp/assets/images/znz.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">sshをproxy経由でつなぐ</h1>
		<p class="rich-link-card-description">
		ssh を proxy 経由で接続してみることがあったので、connect(connect-proxy)とcorkscrewを試してみたのですが、corkscrew は使わない方が良いです。
		</p>
		<p class="rich-link-href">
		https://blog.n-z.jp/blog/2018-08-12-ssh-over-proxy.html
		</p>
	</div>
</a></div>



```shell
$ brew install connect

$ export CONNECT_USER=people
$ export CONNECT_PASSWORD=p30pl312E
$ ssh -o ProxyCommand='connect -H ns2.people.co.jp:8080 %h %p '  takizawausr@takizawanavi.jp -vvv
```

パスワード聞かれた!! 繋がった!!

.ssh/config
```config

Host taki
  ProxyCommand env CONNECT_USER=people CONNECT_PASSWORD=p30pl312E connect -H ns2.people.co.jp:8080 %h %p
  HostName takizawanavi.jp
  User takizawausr
```

```shell
$ ssh taki

takizawausr@takizawanavi.jp's password:
```
繋がった!!


### one liner で行ける
```shell
$ ssh -o ProxyCommand='connect -H people@ns2.people.co.jp:8080 %h %p'  takizawausr@takizawanavi.jp -vvv

まず、proxy のパスワード入れる。

次にssh 先のパスワードを入れる。

OK!!
```

