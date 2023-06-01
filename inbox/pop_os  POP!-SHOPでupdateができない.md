---
type: note
---

#pop_os 

---
2022-08-19  09:38

# pop_os  POP!-SHOPでupdateができない


https://support.system76.com/articles/package-manager-pop/

```shell

$ sudo apt clean
$ sudo apt update -m
$ sudo dpkg --configure -a
$ sudo apt install -f
$ sudo apt full-upgrade
$ sudo apt autoremove --purge
```

これでUpdateが無くなった。


apt update -m で
```
E: リポジトリ https://ppa.launchpadcontent.net/system76/pop/ubuntu jammy Release には Release ファイルがありません。
```

/etc/apt/sources.list.d/system76-ubuntu-pop-jammy.list を入れるとこのエラーが出る。
これは
```shell
$ sudo add-apt-repository ppa:system76/pop
```

これでセットされる。


PPA は Parsonal Package Archive で、個人用リポジトリで管理出来るようにするためのもの。

add-apt-repository で追加するとのこと。なので、ppa:system76/pop を追加したのか。

直にリポジトリ情報を指定出来る。

```shell
$ sudo add-apt-repository "deb http://sample.repository/ubuntu distro component"
```

この場合は /etc/apt/source.list に追加される。

リポジトリkeyを取得して保存することで使えるようになるのか。
apt-key add hoge.key

apt-add-repository ppa:[user]/[ppa-name]   この指定仕方だったのか。
これでやると、GPG public key も自動でダウンロードされて登録されるとのこと。

/etc/apt/trusted.gpg.d/system76-ubuntu-pop.gpg  <--ここに出来てた。

削除する。
non
```shell
$ sudo add-apt-repository --remove ppa:system76/pop
```
gpg は残ってた。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://hombre-nuevo.com/linux/linux0020/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://hombre-nuevo.com/wp-content/uploads/2016/02/ubuntu-logo112.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">apt-get updateでエラーが出た（Ubuntu）</h1>
		<p class="rich-link-card-description">
		「apt-get update」コマンドでパッケージを管理し
		</p>
		<p class="rich-link-href">
		https://hombre-nuevo.com/linux/linux0020/
		</p>
	</div>
</a></div>



