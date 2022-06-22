#chrome_book 


http://www.webupd8.org/2016/03/use-gnome-318-google-drive-integration.html

GNOME で Google Drive にアクセス出来るようになってるらしい。

```shell
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install gnome-control-center gnome-online-accounts

$ gnome-control-center
```
online-accounts で Google をクリックして、ログインを許可した。
ファインダーに Google Drive が見える。

けど、これをやる必要ないので、解除して remove しよっか。

なぜなら直にファイルパスで行けたから。

```shell
/var/host/media/fuse/drivefs-9f18a6dbb628ad88051d457be06bfa77/root
```

```shell
$ sudo apt remove gnome-control-center gnome-online-accounts
```
