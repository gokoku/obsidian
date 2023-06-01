#shopify

---
2021-08-20

# オリジナルテーマの作成

https://agata-code.com/programming/original-no2/


https://www.shopify.jp/partners　<- パートナーの登録

ログイン

https://partners.shopify.com/2116059/resources/get_started

ストア管理 -> ストアを追加する

![[Pasted image 20210820152211.png]]

開発ストア -> いろいろ

パスワードはログインと同じくした。

よくわからんが、開発者プレビューを追加はしなかった。

保存。

### API キーの発行

アプリ管理 -> プライベートアプリを管理する-->

![[Pasted image 20210820153520.png]]

プライベートアプリを有効にする。

![[Pasted image 20210820153640.png]]

プライベートアプリを作成する。

非アクティブな Admin API 権限を表示するをクリックすると、リストが出てくる。

このなかから、テーマを読み書きOK にする。

ここで保存をクリックする。

![[Pasted image 20210820154125.png]]

Admin API に API キーやパスワードが発行された。

![[Pasted image 20210820154220.png]]

API キー パスワードをコピーする。

### スターターテーマのインストール

```shell
$ brew tap shopify/shopify
$ brew install themekit

$ theme version
ThemeKit 1.3.0 darwin/amd64
```


ここから新しいテーマをインストールする。

```shell
$ mkdir shopify
$ cd shopify

$ theme new --password=shppa_4ef... --store=xn-09jg4d6i8b9fse6g.myshopify.com --name=people-shop
```

--store は ログインした URL のドメイン

コンソールの方でもテーマが登録された。

![[Pasted image 20210820155949.png]]

アクションで公開するにする。

![[Pasted image 20210820160218.png]]

ストアを見る。

![[Pasted image 20210820160751.png]]

new テーマ って寂しい限り。

![[Pasted image 20210820160817.png]]

ローカルでターミナルから
```shell
$ theme watch --allow-live
```

をやると、この公開ページのコード変更が反映される。

```shell
$ theme deploy   全ファイル反映
```

