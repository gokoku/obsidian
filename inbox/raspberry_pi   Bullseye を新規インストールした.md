#raspberry_pi 

---
2022-03-11  09:32

# raspberry_pi   Bullseye を新規インストールした

![[スクリーンショット 2022-03-11 9.33.02.png | 500]]

何回やっても失敗するので、変だなーと。
歯車マークの設定をしたらうまく起動するようになった。

advance とあるが、Bullseye では必須かも。

![[Pasted image 20220311093659.png | 500]]
初回起動するときにはキーボードとマウスだけにするのも必須かも。
もちろんGPIO も外して、USBメモリも外しておく。

これでやっと普通に起動するようになった。

```shell
$ sudo apt update
$ sudo apt upgrade
```
この後、必ず再起動する。

~/.Xauthority の中身を空にする。
```shell
$ :> .Xauthority
```







