---
type: note
---

#pop_os 

---
2022-05-10  10:58

# pop_os Google-Drive を auto mount したい

google-drive を自動マウントする記事は今でも、_google_-_drive_-ocamlfuse ばかりだ。

せっかく OS レベルでマウント機能があるからそれを使いたいのに。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://askubuntu.com/questions/1350137/mount-google-drive-automatically-at-startup-as-in-nautilus" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://cdn.sstatic.net/Sites/askubuntu/Img/apple-touch-icon@2.png?v=c492c9229955')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Mount Google Drive automatically at startup (as in Nautilus)</h1>
		<p class="rich-link-card-description">
		I use the mount point /run/user/&lt;user_id&gt;/gvfs/google-drive:host=&lt;host&gt;\,user=&lt;username&gt;/&lt;folder_id&gt;/ in an automated backup script to copy some files on Google Drive regula...
		</p>
		<p class="rich-link-href">
		https://askubuntu.com/questions/1350137/mount-google-drive-automatically-at-startup-as-in-nautilus
		</p>
	</div>
</a></div>

```shell
$ gio mount google-drive://address@gmail.com
```

これでイケる。

super + A --> System --> 自動起動するアプリケーション
に登録する。

![[スクリーンショット 2022-05-10 11.08.39.png| 500]]

![[スクリーンショット 2022-05-10 11.10.44.png | 400]]

出来た。
