---
type: note
---

#mysql #command

---
2022-05-02  14:43

# mysql  SSH のポートフォワーディングで MySQL に接続する


<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.tam-tam.co.jp/tipsnote/program/post14579.html" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.tam-tam.co.jp/tipsnote/wpdata/wp-content/uploads/2018/01/sshportfowarding-1024x538.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">SSH のポートフォワーディングを使って MySQL に接続する ｜ Tips Note by TAM</h1>
		<p class="rich-link-card-description">
		TAM のテクニカルチームがお届けする WEB技術ブログ！
		</p>
		<p class="rich-link-href">
		https://www.tam-tam.co.jp/tipsnote/program/post14579.html
		</p>
	</div>
</a></div>
.ssh/config で http proxy を介す。

![[ssh 2022-05-02 17.59.30.excalidraw | 600]]

```config
Host taki
  Proxycommand env CONNECT_USER=people CONNECT_PASSWORD=p30pl312E /usr/local/bin/connect -H ns2.people.co.jp:8080 %h %p
  HostName takizawanavi.jp
  User takizawausr
```

ssh で db のあるサーバに入り込み、**ポートフォワーディング** をする。
これは、MySQL のポートをMySQL側から変える方向だから、フォワーディングなのかな。

ssh のオプションで 
 - -N がポートフォワーディングを有効にする。
 - -L で 変えるポート : ローカル側 : MySQLポート という感じに設定する。
 - -f でバックグラウンドにまわせる。


```shell
$ ssh -NL 127.0.0.1:9999:127.0.0.1:3306 taki -vvv
```
これで、転送設定がされた。後は、mysql クライアントでやり取り出来る。

```sell
$ mysql -h 127.0.0.1 -P 9999 -u DB名 -p
パスワード

mysql>
```
行きました!!

# Sequel Ace で接続出来た
てゆーか、Ace はこれでしか接続出来ない?!

![[Pasted image 20220502163723.png | 400]]
つまり、Sequel Ace の中では、http proxy client コマンドが動かないので、外側で仲介接続を確立させて、後は、普通に MySQL 接続するということか。


## JSGCS の MariaDB

```config
Host ganport
Proxycommand env CONNECT_USER=people CONNECT_PASSWORD=p30pl312E connect -H ns2.people.co.jp:8080 %h %p
hostname 210.152.116.228
Port 10220
User pmorioka
```

ssh では port 10220 でないと、mariaDB に接続できないようになっている。

```shell
$ ssh -NL 127.0.0.1:3306:127.0.0.1:3306 ganport -vvv
```


![[Pasted image 20220912143327.png]]

![[Pasted image 20220912143342.png]]


