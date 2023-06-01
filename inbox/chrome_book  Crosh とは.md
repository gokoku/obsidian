そし#chrome_book 

Chrome ブラウザで Ctrl + Alt + t を押すと crosh が起動する。

コンテナのホストVMを管理したり、VMを立てたりVMにログインできたりする。

```jsx
$ vmc list   [[VM]]一覧を取得
termina            # 標準状態のVMらしい
$ vsh termina      # termina にログイン

ログイン後、コンテナを操作したり出来る。

> lxc list         # コンテナの一覧
> lxc images:debian/bullseye new-container   # コンテナの作成
> lxc exec new-container -- /bin/bash        # コンテナにログイン
```

## LXC とは

linux containers のこと。

今どき仮想化技術。Docker にあたる。

chrome book に使われてるのが Crostini (クロスティーニ) そのコアが、LXC とのこと。


---

```shell
$ lxc config device add <Container-name> <device-name> usb vendorid=<id> productid=<id>
```

コンテナ名は
```shell
$ lxc list

penguin
```


vendorid と productid は
```shell
$ lsusb -v

idVendor        0x21c4
idProduct       0x0cd1
iManufacturer   Lexar

```

device name は任意 Lexar にした。
```shell
$ lxc config device add penguin Lexar usb vendorid=21c4 productid=0cd1
Device Lexar added to penguin



取り除く
$ lxc config device remove penguin Lexar

```

どこにくっついたというのだ??

```shell
$ lxc config device list penguin

Lexar
```
あるけど、どうやってコンテナの中からアクセス出来る??



---

# Developer mode で crosh

Ctrl + Alt + t からの >shell 

USB がマウントされている。
```shell
$ df -h

/dev/sda1      /media/removable/chrome_book_1
```

これをコンテナに渡せば
