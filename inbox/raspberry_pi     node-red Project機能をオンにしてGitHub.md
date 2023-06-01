#raspberry_pi  #node-red 


~/.node-red/settings.js

```bash
editorTheme: {
	projects: {
		enabled: true
```

enabled : false から true にする。

node-red 再起動

```bash
$ sudo systemctl enable nodered.service
```

![](image-kndvckih.png)

ここでコミットする。

ブランチも変更出来る。

![](image-kndvd2kr.png)

これを社内 Gitlab に上げたい。

Gitlab に空リポジトリを作った。

![](image-kndvdfwt.png)

node-red のメニューからプロジェクトの設定。

![](image-kndvdvkr.png)

リモート URL を設定した。

![](image-kndvenpu.png)

コミット履歴の上下行ったり来たりのアイコンをクリックしたらリモート系が進んだ。

![](image-kndvf43a.png)

認証通して

![](image-kndvfno5.png)

リモートなしのところをクリックして、

![](image-kndvgfk3.png)

master 指定して、作成をすると。

push 出来るようになった。

![](image-kndvgw1z.png)

なんか、コマンドのように操作がよくわからん。

ブランチをマスターにマージするとかよくわからん。

めんどくさいから master 一本道にする？

一人プルリクとかめんどくさいしー