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

## Bullseye で node-red core が入らない

node に .nvm を使ってインストールした。

```
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

.bashrc
```
export NVM_DIR="$HOME/.nvm"
export NVM_SYMLINK_CURRENT=true
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```
***NVM_SYMLINK_CURRENT*** は true にする。

node のインストール。
```shell
$ nvm ls-remote
$ nvm install 18
$ nvm install node  # latest version

$ nvm use 18

```


npm を asdf とか nvm で入れてると sudo npm で見つからないと言われる。
npm のパスを無理矢理通すことにした。
```shell
$ sudo visudo
```

```
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/home/people/.nvm/current/bin"
```
.nvm のパスを追加して上のスクリプトを回した。


node と node-red-pi が見つからないので、/usr/local/bin にリンクを貼った。
```shell
$ ln -s /home/people/.nvm/current/bin/node /usr/local/bin/
$ ln -s /home/poeple/.nvm/curent/bin/node-red-pi /usr/local/bin/

念の為
$ ln -s /home/people/.nvm/current/bin/node-red /usr/local/bin/
```






# 起動
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

