---
type: note
---

#chrome_book #ubuntu

---
2022-05-06  10:48

# chrome_book Acer c720 に Ubuntu をクリーンインストールする


<div class="rich-link-card-container"><a class="rich-link-card" href="https://izumisy.work/entry/2021/03/14/195505" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://hatenablog-parts.com/embed?url=https%3A%2F%2Fizumisy.work%2Fentry%2F2021%2F03%2F14%2F195505')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Acer C720のSSDを換装してUbuntu 20.04 LTSをクリーンインストールする - Runner in the High</h1>
		<p class="rich-link-card-description">
		Acer C720というChromebookが手元にあるのだが、しばらく前にサポートが切れたので気合を入れてUbuntuをクリーンインストールする。 ついでにSSDも換装する。 acer ChromeBook C720 (日本正規品)発売日: 2014/11/13メディア: Personal Computers Sea…
		</p>
		<p class="rich-link-href">
		https://izumisy.work/entry/2021/03/14/195505
		</p>
	</div>
</a></div>


USBブートを有効にするとな?

開発者モードに入って、chronos ユーザで
```shell
# USBブートを有効にする
$ sudo su -
# crossystem dev_boot_usb=1 dev_boot_altfw=1

prease use 'dev_boot_altfw' instead of 'dev_boot_legacy' と言われたので。

再起動。
白いブートスプラッシュ画面で、Ctrl + L を押す。

実はひと処理必要なものがあった。
その後で上手くいった。


# CTRL+Lを押さなくても勝手にUbuntuが起動するようにする
$ sudo /usr/share/vboot/bin/set_gbb_flags.sh 0x489
```

C720 にハードの書き込み防止ビスがあるらしい。
![](https://cdn-ak.f.st-hatena.com/images/fotolife/I/IzumiSy/20210314/20210314192214.png)

⑦とのこと。

8G程度の本体ストレージを使うことはないので、USB ブートだけでいいかな。