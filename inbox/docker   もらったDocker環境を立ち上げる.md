#docker

---
2022-02-04  12:00

# docker   もらったDocker環境を立ち上げる

Iwate ITさんから頂いたDockerを立ち上げる。

```shell
$ cd to/path
$ docker-compose up
```

起動しているサービスを見る。

```shell
$ docker ps

CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                NAMES
486e0b739b2f   iwate_it_web   "/sbin/init"             57 minutes ago   Up 57 minutes   0.0.0.0:18080->80/tcp                webrep01_web
24098e55124d   mysql:5.7.33   "docker-entrypoint.s…"   57 minutes ago   Up 57 minutes   33060/tcp, 0.0.0.0:13306->3306/tcp   webrep01_db
```

### mysql

0.0.0.0:13306 -> 3306/tcp

これは 0.0.0.0 で誰でも見れる状態なので、ホストのループバックでアクセスできる。

![[Pasted image 20220204130032.png]]

これで接続出来た。

中にも入れる。

```shell
$ docker exec -it webrep01_db bash
```


## apache

```shell:/etc/hosts
127.0.0.1 ksw.webrep.v2.docker
```

http://ksw.webrep.v2.docker/test.php はアクセスできる。

![[Pasted image 20220204130458.png]]

bootstrap.php がどうのと言ってる。

```shell
$ docker exec -it webrep01_web bash

# cat /etc/os-release
CentOS 7

# systemctl status httpd
Failed to get D-Bus connection
```

https://qiita.com/NaoyaMiyagawa/items/22a870112377a91e1c99
<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/NaoyaMiyagawa/items/22a870112377a91e1c99" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-user-contents.imgix.net%2Fhttps%253A%252F%252Fcdn.qiita.com%252Fassets%252Fpublic%252Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png%3Fixlib%3Drb-4.0.0%26w%3D1200%26mark64%3DaHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9RG9ja2VyJTIwZm9yJTIwbWFjJTIwJTI4dmVyJTIwNC4zJUU0JUJCJUE1JUU5JTk5JThEJTI5JTIwJUUzJTgxJUE3JUUzJTgwJThDRmFpbGVkJTIwdG8lMjBnZXQlMjBELUJ1cyUyMGNvbm5lY3Rpb24lRTMlODAlOEQlRTMlODIlQTglRTMlODMlQTklRTMlODMlQkMlRTMlODElOEMlRTUlODclQkElRTMlODElOUYlRTMlODElQTglRTMlODElOEQlRTMlODElQUUlRTUlQUYlQkUlRTUlODclQTYlRTYlQjMlOTUmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT01NiZ0eHQtY2xpcD1lbGxpcHNpcyZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTg5MGFjMWQ2Y2MzMmNiYzIxNzJkMzk0MjQxNmY4ZWMx%26mark-x%3D142%26mark-y%3D112%26s%3D0674dd30098649bc34ec770d7703ab61?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwTmFveWFNaXlhZ2F3YSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9MjQ0YTRiMTQ3Y2FmZDJlZWUwNjk4NThkOGJjYTJkZGY&mark-x=142&mark-y=491&s=462f3740e709ebbd7a38f047ffe4716c')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Docker for mac (ver 4.3以降) で「Failed to get D-Bus connection」エラーが出たときの対処法 - Qiita</h1>
		<p class="rich-link-card-description">
		こんにちは、みやがわです。  Docker for macを使って開発をしています。 2021年12月に入ってAnsibleでの環境構築時に、題目のD-busエラーなるものが出て困ったので、調査しました。  ※ 12月より以前から出て...
		</p>
		<p class="rich-link-href">
		https://qiita.com/NaoyaMiyagawa/items/22a870112377a91e1c99
		</p>
	</div>
</a></div>

```json:~/Library/Group\ Containers/group.com.docker/settings.json
// ~/Library/Group\ Containers/group.com.docker/settings.json

  "deprecatedCgroupv1": true,
```

false を true にした。

Docker 再起動。

```shell
# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: active (running) since 金 2022-02-04 11:41:18 JST; 1h 28min ago
     Docs: man:httpd(8)
```

ちゃんと動いてる。

cakePHP の環境になってないのだな。
調べる。



### 因みに Docker の IP を知りたい

```shell
$ docker inspect webrep01_web


                    "IPAddress": "172.20.0.3",

$ docker inspect webrep01_db

                    "IPAddress": "172.20.0.2",
```



