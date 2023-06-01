#virtual_box

---
2021-11-13

# VirtualBox またまた事始め

windows のアプリ G keyboard の設定だよ　を使いたくて VirtualBox にすると思ったが、忘れてるので、再入門。

```shell

$ brew install --cask virtualbox
$ brew install --cask virtualbox-extension-pack
```

virtualbox-extension-pack は /usr/local/Caskroom/virtualbox-extension-pack/6.1.28/ にあったので、ダブルクリックしてインストールした。

windows の OVA をダウンロードしたい。

ここを参考にする

https://qiita.com/hinataysi29734/items/d4e48ca673bad2f5ea03

検証用Windows OVA をまんまダウンロードする

https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/

![[Pasted image 20211113233915.png]]

このまま、ovf をダブルクリックすると、vmdk の方もimportしてくれてるみたい。

パスワード : Passw0rd!

### 新規マシン

新規から名前をwin10 メモリ8G 仮想ハードディスク VDI 可変サイズ。

### 設定

ストレージ->空担ってる光学ドライブで仮想ディスクファイルを選択する。

![[Pasted image 20211113231010.png]]

追加

![[Pasted image 20211113231146.png]]

起動

マウスが捕まって出られなかった。Left コマンド　押すと、解放される。mouse integrate チェック外してるんだけど?







