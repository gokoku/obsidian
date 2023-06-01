---
type: note
---

#chrome_book #pop_os 

---
2022-05-06  23:52

# chrome_book Pop!_OS を入れた!


<div class="rich-link-card-container"><a class="rich-link-card" href="https://pop.system76.com/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://pop.system76.com/apple-touch-icon.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Pop!_OS by System76</h1>
		<p class="rich-link-card-description">
		Imagine an OS for the software developer, maker and computer science professional who uses their computer as a tool to discover and create. Welcome to Pop!_OS.
		</p>
		<p class="rich-link-href">
		https://pop.system76.com/
		</p>
	</div>
</a></div>


DOWNLOADで

![[Pasted image 20220506235356.png | 300]]

これをMint に DL した。
Mint でこのファイルを右クリックすると、「ブート可能なUSBメモリの作成」とあるので、もうこの手の iso ファイルは Linux でやっちまった方が超楽だ。

## POP!_OS install
後は、installer が動くので、install 先を USB の 216G の SSD にして、暗号化はなしでやった。(暗号化したら失敗した)

再起動して、Ctrl + L ですかさず ESC で BUFFALO の SSD を選択したら、この通り!!
メチャ可愛い!!
chromebook 生まれ変わったよ!!

![[2022-05-07_00-09.png]]

[[chrome_book  USBブートがしたい]]

これで、POP!_OS マシンになった。

## POP!_OS ならではのやつ
- Tile Windows
	- Window がタイル状に配置される摩訶不思議なインタフェース
	- 例外でフローティングさせたいアプリを指定出来る Floating Window Exceptions

## やりたいこと
- keyboard の配置で ctrl super meta キーを変える
	- [[pop_os キーボードのレイアウトを変更する 1]]
	- [[pop_os キーボードのレイアウトを変更する 2]]
- 日本語入力出来るようにする
	- 言語サポートで日本語パッケージのインストールしてある --> ログアウト
	- キーボードの入力ソース --> 日本をクリックしたら、日本語(Mozc) が出たので追加
	- かな英数キーで日本語入力ができるようになった。なってたのかな?
	- かな入力にしたい --> Mozcプロパティで指定
	-  [[pop_os  日本語環境何とかする]]  何ともしなかったか
- Google Drive からの KeePassXC
	- [[pop_os  KeePassXC をセットする]]
- ssh 秘密鍵の設置
- Homebrew を入れる
	- [[mint Linuxbrew を入れる]]
- asdf を入れる
	- [[raspberry_pi で asdf-vm を導入する]]
```shell
	$ asdf plugin add java
	$ asdf install java openjdk-18.0.1
	$ asdf global java openjdk-18.0.1
	$ asdf reshim
```

- Docker を入れる
	- [[pop_os  Docker を入れてみる]]
- zshell を設置する
	- [[pop_os  Zsh を設置した]]
- .emacs.d を Github から cloneして設置する
```shell
$ git clone git@github.com:gokoku/.emacs.d.git

$ emacs

M-x package-list-packages

インストールパッケージ
use-package
indent-guide
auto-async-byte-compile
paredit
```

- Flatpak パッケージ管理を入れる<--既に入ってる
- VScode を入れる --> Pop!_shop で入れた(これ Flatpak だ !!)
	- Setting Sync を install
	- Github に接続
	- 設定をDOWNLOAD で後は、それなりに承認して
- POP!_Shop からインストール(~/.var/app の中に入る)
	- Blender を入れた
	- MEGAsync 入れた
	- Obsidian 入れて、MEGSAsync と同期する
		- plugin を入れて設定する

- tmux を入れて設定する
	- [[raspberry_pi  Tmux の設定]]







