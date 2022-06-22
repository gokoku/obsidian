#blender 


https://flashforge.co.jp/

FLASHFORGE というものらしい。
機種 : Adventurer 3

ソフトのダウンロード、Mac も Linux も対応してる。
https://flashforge.co.jp/support/

Flashprint

https://flashforge.com/download-center/63
Linux はここにあった。


### Blender のデータの作り方

#### 下準備

カメラと光源は削除。

単位を Meters にする。

![](image-kmy94dyl.png)

Length を Millimeters にすると単位がmmになる。

グリッドをcmにしてみる。
https://ishigi.com/blender-units-cm/

![](image-kmy9zuz2.png)
Scale を0.01 にする

---
https://note.com/kawakoshi_shi/n/nc6e0c1b54a5f

どうも、FlashPrint では Blender の単位が間違って伝えられるらしい。
メートルの数字がそのままmmに伝わるとのこと。

なので、export のときにスケールを 1000倍にするとうまくいくとのこと。
![](2021-04-01-11-49-37-kmya6ms3.png)


---

実物の 10倍 にして 100倍にするのがいい。
実物大だと、取り回しが無理。

40cm くらいにして、
![](image-ko18gu2o.png)

100倍 の stl 形式
![](image-ko18hccx.png)

## FlashPrint

stl を読み込ませて、スライス実行する。

![[Pasted image 20220518094440.png]]

USB メモリに移す。
OUTPUT ファイルどうやるんだろう。



<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/ideagear/items/d7068b14a9a11d1032e3" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9M0QlRTMlODMlOTclRTMlODMlQUElRTMlODMlQjMlRTMlODIlQkYlRTMlODMlQkMlMjhEcmVhbWVyJTI5JUUzJTgxJUFCJUUzJTgyJTg4JUUzJTgyJThCU1RMJUUzJTgzJTk1JUUzJTgyJUExJUUzJTgyJUE0JUUzJTgzJUFCJUUzJTgxJUFFJUU1JTg3JUJBJUU1JThBJTlCJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz04NTg2ODA5YjNjYTQ0NjZiZGM4YTQyMmI5NjlhODZjOQ&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwaWRlYWdlYXImdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPWQwMjY3ZDdlNTVmMGJlZDEyNDRlM2I4ZGIyYTc3MjFj&blend-x=142&blend-y=491&blend-mode=normal&s=5417b3291a2523ed67f1711a35932e7d')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">3Dプリンター(Dreamer)によるSTLファイルの出力 - Qiita</h1>
		<p class="rich-link-card-description">
		こんにちは．Ideagearの林です． 今回はSTLファイルをスライサーソフトを経由して3Dプリンターで出力，の流れについてまとめたいと思います． 環境  3Dプリンター：Dreamer スライサーソフト：FlashPrint(F...
		</p>
		<p class="rich-link-href">
		https://qiita.com/ideagear/items/d7068b14a9a11d1032e3
		</p>
	</div>
</a></div>

Gcode を作成するそうです。

ローカルに保存で gx ファイルが出来た。

これを USBメモリに入れる。
Fatx とかでフォーマットし直した。


