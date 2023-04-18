#js 

puppeteer がやたら落ちる。
Big sur にしてから。

パッケージが古くなってるのだろうか。

```shell
$ npm outdated
```
これで package.json と今のバージョンがわかる。

全部古い
どうするか、
npm-check-updates を使うと便利らしい。

https://qiita.com/sugurutakahashi12345/items/df736ddaf65c244e1b4f


インストール
```shell
$ npm -g i npm-check-updates
```

1. アップデートの確認をする。
```shell
$ cd somedir
$ ncu

Checking /Users/george/bin/puppeteer/package.json
[====================] 12/12 100%

 colors                   ^1.3.2  →   ^1.4.0
 dotenv                   ^6.1.0  →   ^8.2.0
 moment                  ^2.22.2  →  ^2.29.1
 puppeteer                ^1.9.0  →   ^8.0.0
 eslint                   ^5.8.0  →  ^7.21.0
 eslint-config-google    ^0.11.0  →  ^0.14.0
 eslint-config-standard  ^12.0.0  →  ^16.0.2
 eslint-plugin-import    ^2.14.0  →  ^2.22.1
 eslint-plugin-node       ^8.0.0  →  ^11.1.0
 eslint-plugin-promise    ^4.0.1  →   ^4.3.1
 eslint-plugin-standard   ^4.0.0  →   ^4.1.0

Run ncu -u to upgrade package.json

```

2. package.json の更新をしてくれる。

```shell
$ ncu -u

Upgrading /Users/george/bin/puppeteer/package.json
[====================] 12/12 100%

 colors                   ^1.3.2  →   ^1.4.0
 dotenv                   ^6.1.0  →   ^8.2.0
 moment                  ^2.22.2  →  ^2.29.1
 puppeteer                ^1.9.0  →   ^8.0.0
 eslint                   ^5.8.0  →  ^7.21.0
 eslint-config-google    ^0.11.0  →  ^0.14.0
 eslint-config-standard  ^12.0.0  →  ^16.0.2
 eslint-plugin-import    ^2.14.0  →  ^2.22.1
 eslint-plugin-node       ^8.0.0  →  ^11.1.0
 eslint-plugin-promise    ^4.0.1  →   ^4.3.1
 eslint-plugin-standard   ^4.0.0  →   ^4.1.0

Run npm install to install new versions.

```

3. 後はアップデートインストール

```shell
 npm install
```

ガンとバージョンが上がった。
