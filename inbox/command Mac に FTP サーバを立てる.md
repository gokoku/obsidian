#command

---
2022-04-20  09:33

# command Mac に FTP サーバを立てる
peopleサーバに FTP サーバを立てて、Canon printer からスキャンファイルを転送したい。

```shell
$ brew install pure-ftpd

起動
$ brew services restart pure-ftpd
```

/usr/local/etc/pure-fptd.conf に設定する。

```config
PureDB                       /usr/local/etc/pureftpd.pdb
```

とした。

```shell
$ touch /usr/local/etc/pureftpd.pdb # dbファイルを作る

$ brew services restart pure-ftpd   # 再起動

$ 
```


```shell
uid と gid を知る

$ id
uid=501(people2) gid=20(staff)....

$ 
```

https://dexlab.net/pukiwiki/index.php?Memo/Linux/pure-ftpd


#### osx は結構特殊
homebrew で入れるので、root ではないユーザで起動出来る。
pure-pw でユーザを追加するのに root で作ると ftpd が触れないようだ。
では、sudo 付けないで pure-pw でユーザ追加すると、Illegal だと怒られる。

https://superuser.com/questions/1541572/cannot-log-in-using-pure-ftpd

/etc/pam.d/pure-ftpd を sudo で作る。

```
# pure-ftpd: auth account password session
auth       required       pam_opendirectory.so
account    required       pam_permit.so
password   required       pam_deny.so
session    required       pam_permit.so
```

Set pam configuration

```shwll
$ sudo /usr/local/sbin/pure-ftpd -lpam -B
```

これで起動する。
```shell
$ brew services restart pure-ftpd
```



### 確認のために ftp クライアントを入れる


<div class="rich-link-card-container"><a class="rich-link-card" href="https://maku77.github.io/mac/ftp.html" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://maku77.github.io/assets/img/logo-mac.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">昔ながらの ftp コマンドを使用できるようにする | まくまくMacノート</h1>
		<p class="rich-link-card-description">
		天才星人まくのMacノート
		</p>
		<p class="rich-link-href">
		https://maku77.github.io/mac/ftp.html
		</p>
	</div>
</a></div>

```shell
$ brew install tnftp

$ ftp user@example.local
ftp> cd サーバーの cd 先
ftp> lcd クライアントの作業フォルダ
ftp> put icon.png
```

オッケー!!!!

Cannon で取る大量のスキャンデータを転送出来るか。

管理者設定からpw入れる--> ファックス・スキャン転送ボックス --> FTP

IP : 192.168.20.16
ファイルパス : \/current_HDD1\/Projects/Scan


