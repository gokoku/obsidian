#chrome_book 

https://qiita.com/htnk/items/6458ec6412210406486b

Crostini
公式のLinux環境作成機能 KVMとLXCを使用し、DebianやArch Linux、CentOS等を起動できる Sommelierというウィンドウマネージャを使ってGUIアプリを表示する 仮想マシン + コンテナなので動作が重い Chromebookのハードウェアの使用に制限がある デベロッパーモード不要

Crouton
非公式のLinux環境作成スクリプト chrootを使用し、Ubuntu、Debian、Kali Linuxを起動できる xiwiというChrome拡張機能を使ってGUIアプリを表示する xorgを使ってChrome OSと切り離されたGUI表示を行うことも可能 Chromebookに接続したUSB機器に直接アクセスできる デベロッパーモード必要

Chromebrew
非公式のChrome OS用パッケージマネージャ 他のLinuxディストリビューションを入れる訳ではなく、Chrome OSに直接パッケージをインストールする Sommelierというウィンドウマネージャを使ってGUIアプリを表示する デベロッパーモード必要


```shell
$ sudo apt update
$ sudo apt upgrade
$ sudo apt dist-upgrade
```

