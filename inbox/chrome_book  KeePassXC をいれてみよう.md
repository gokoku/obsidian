#chrome_book 

Google Drive にファイルでアクセス出来ることがわかったので、

KeePassXC を入れてみよう。



```shell
$ sudo add-apt-repository ppa:phoerious/keepassxc
```
コマンドが見つからないという。

```shell
$ sudo apt install software-properties-common
```

```shell
$ sudo add-apt-repository ppa:phoerious/keepassxc
# うまくいった!!!

$ sudo apt update
$ sudo apt install keepassxc
```

日本語でうまく動いてくれてる!!

既存のデータベースのパス
```shell
/var/host/media/fuse/drivefs-9f18a6dbb628ad88051d457be06bfa77/root/one/one.kdbx
```

バッチリだ!!

