---
type: note
---

#asdf

---
2023-04-27  10:19

# asdf  バージョンがあるのにも関わらずそのバージョンを入れてくれと言われて動かなかったとき

nodejs で yarn を入れて
```shell
$ asdf current nodejs
16.20.1

$ npm install yarn -g
```

動かすと、16.20.1 をこのように入れてくださいと言われて動かない現象

```shell
$ yarn install

No preset version installed for command yarn
Please install a version by running one of the following:

asdf install nodejs 16.20.0

or add one of the following versions in your config file at /Users/george/repository/tono_reservation_system/.tool-versions
nodejs 19.8.1
```

拉致があかない!!!
結構このパターンよく見る。
elixir の時もこれで刺さって動かなかった。

## 解決方法

プラグインの入れ直しをする。
```shell
$ asdf plugin-remove nodejs
$ asdf plugin-add nodejs

$ asdf install nodejs 16.20.0
$ asdf global nodejs 16.20.0
$ npm install yarn -g
$ yarn

running!!!!! OK!!
```

その他、chatGPT で教えてもらったこと。

プラグインのアップデート
```shell
$ asdf plugin-update
```

asdf の再インストール
```shell
$ asdf uninstall nodejs
$ asdf plugin-remove asdf
$ -rf ~/.asdf
$ git cllone https://github.com/asdf-vm/asdf.git ~/.asdf -branch v0.8.0
```

パッケージを手動でインストールする

```shell
$ curl -L -O https://github.com/パッケージ名/archive/バージョン番号.tar.gz
$ tar xvfz バージョン番号.tar.gz
$ cd バージョン番号
$ ./configure
$ make
$ sudo make install
```

