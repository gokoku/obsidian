---
type: note
---

#ubuntu #obsidian

---
2023-02-09  23:05

# ubuntu  Obsidian で google-drive-ocamlfuse を使う

google.6809@gmailcom のドライブはファインダーでは見えている。
が、ターミナルで開いてみるとディレクトリ名が変なことになっている。

ファインダー越しでは普通のファイル名に見えてても、実際の名前は違うようだ。


![[Pasted image 20230209231008.png]]

![[Pasted image 20230209231209.png]]
なんか変わってしまって　Obsidian から読めなくなったようだ。

なので、google-drive-ocamlfuse を使ってみることにしたらうまくいった

<div class="rich-link-card-container"><a class="rich-link-card" href="https://linuxhint.com/install-google-drive-on-ubuntu-22-04/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://linuxhint.com/wp-content/uploads/2021/09/cropped-512x512_linuxhint-192x192.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">How to Install Google Drive on Ubuntu 22.04</h1>
		<p class="rich-link-card-description">
		To install Google Drive on Ubuntu 22.04, use either GNOME Online Accounts framework or terminal by executing “sudo apt install google-drive-ocamlfuse” command.
		</p>
		<p class="rich-link-href">
		https://linuxhint.com/install-google-drive-on-ubuntu-22-04/
		</p>
	</div>
</a></div>



```shell
$ sudo add-apt-repository ppa:alessandro-strada/ppa
$ sudo apt update
$ sudo apt install google-drive-ocamlfuse
$ google-drive-ocamlfuse
$ mkdir -v ~/myGoogleDrive
$ google-drive-ocamlfuse ~/myGoogleDrive

```




## 起動時にマウントさせたい

Startup Applications Preferences でスクリプトをセットする。
![[Pasted image 20230210225932.png]]

~/bin/mount_google_drive スクリプトをここに加えた。

```shell
#!/usr/bin/bash                                                                                

/usr/bin/google-drive-ocamlfuse $HOME/myGoogleDrive
```







# 何故読めないのか

Google Drive API から Client ID と Secret を取得する必要があるらしい。

ということで ocamlfuse が出る。

