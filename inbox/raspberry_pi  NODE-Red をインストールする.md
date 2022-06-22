#raspberry_pi #node-red 

---
2022-03-10  11:53

# raspberry_pi  NODE-Red をインストールする


**Bullseye ではもう入ってる。**


https://nodered.jp/docs/getting-started/raspberrypi
<div class="rich-link-card-container"><a class="rich-link-card" href="https://nodered.jp/docs/getting-started/raspberrypi" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://nodered.jp/favicon.ico')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Raspberry Piで実行する : Node-RED日本ユーザ会</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://nodered.jp/docs/getting-started/raspberrypi
		</p>
	</div>
</a></div>


```shell
$ bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)
```

これが一番楽なようだ。
LTS な ここNode も入れてくれる。

```
$ node-red-stop
$ node-red-start
$ node-red-log

$ sudo systemctl enable nodered.service

$ sudo systemctl disable nodered.service
```

### Project を導入する


<div class="rich-link-card-container"><a class="rich-link-card" href="https://nodered.jp/docs/user-guide/projects/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://nodered.jp/favicon.ico')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">プロジェクト : Node-RED日本ユーザ会</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://nodered.jp/docs/user-guide/projects/
		</p>
	</div>
</a></div>

.node-red/setting.js

```js:.node-red/setting.js
projects: {
   enabled: true  <--- false からtrue にする
}
```

![[Pasted image 20220310131141.png]]
再起動

![[Pasted image 20220310131325.png]]

gitlab にあげている watson-ai.git と同じ名前にして、中を入れ替えてみる。

甦った。

