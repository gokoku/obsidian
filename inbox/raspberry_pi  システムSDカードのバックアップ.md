#raspberry_pi

---
2021-05-31

# rpi-clone を使う
https://github.com/billw2/rpi-clone

rpi-clone を SDカードシステムの標準バックアップにすることにしよう。


USB カードで微妙に容量が違うので、ちょっと大きめな容量が元のとき、普通の容量にバックアップ失敗現象に遭遇する。

この rpi-clone はファイルでやるので、容量差は関係ない上に、
バイト単位でコピーしない分とても速い。

```shell
$ git clone https://github.com/billw2/rpi-clone.git
$ cd rpi-clone
$ sudo cp rpi-clone rpi-clone-setup /usr/local/sbin
```

USB3.0 のカードtoUSBアダプタを USB2.0 の口に入れると、電力不足か raspberry pi が落ちる。たぶんディスプレイに電力が取られる。

なので、USB3.0 アダプタは USB3.0 の方に入れる。

# バックアップの取り方

コピー先のデバイスを確認する。

コピー先はSDカード。USBアダプタに入れてセットする。

```shell
$ lsblk

sda      28.9G
```

これっぽいってゆーかこれだ。

```shell
$ sudo rpi-clone sda

yes
yes
yes
```

これで出来たクローンがそのまま raspberry pi のシステムSDカードになった。