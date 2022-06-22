---
type: note
---

#pop_os 

---
2022-05-11  09:57

# pop_os キーボードのレイアウトを変更する 2

xmodmap でキーのリマップしてるが、時々効かなくなったりして困る。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://github.com/sezanzeb/input-remapper" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://repository-images.githubusercontent.com/307515577/d1901780-a28a-11eb-9b09-092a7e9ef355')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">GitHub - sezanzeb/input-remapper: 🎮 An easy to use tool to change the mapping of your input device buttons.</h1>
		<p class="rich-link-card-description">
		🎮 An easy to use tool to change the mapping of your input device buttons. - GitHub - sezanzeb/input-remapper: 🎮 An easy to use tool to change the mapping of your input device buttons.
		</p>
		<p class="rich-link-href">
		https://github.com/sezanzeb/input-remapper
		</p>
	</div>
</a></div>


input-remapper を入れてみる。

```shell
$ sudo apt install git python3-setuptools gettext
$ git clone https://github.com/sezanzeb/input-remapper.git
$ cd input-remapper && ./scripts/build.sh
$ sudo apt install ./dist/input-remapper-1.4.2.deb
```

input-remapper アプリで起動する。

Device を選択するところで、keyboard を選ぶ。

![[Pasted image 20220511110456.png]]

Change Key をクリックして、Super L (虫眼鏡)を押す。

![[Pasted image 20220511110631.png]]

何にチェンジするかを打ち込むのだが、サジェスチョンが掛かって助かる。

![[Pasted image 20220511110853.png]]

次は Ctrl L を Super L にしたい。
new entry をクリックすると次に登録出来る。

こんな感じで3つの登録をする

![[Pasted image 20220511113014.png]]
![[Pasted image 20220511113033.png]]
![[Pasted image 20220511113049.png]]

気をつけるのは、「今押したキーを是是にする」という選択の仕方。
てゆーか最初からこうしたかった。

LGTM !!! Looks Good To Me !!!
てゆーか最高。

### iMac 用には zenkaku_hankaku をかなに割り当てると日本語が入力できる

![[Pasted image 20220513221154.png]]


