#raspberry_pi 

DOHC でぶら下げて、ホスト名で接続するようにする。

ホスト名の確認

```bash
$ hostname
people
```

ネットワーク名は people.local

sshの接続

```bash
$ ssh pi@people.local
```

DOHCで割り振られた IP の確認

```bash
$ ifconfig
wlan0:       <---が wifi のIP
```

# 画面共有

Raspberry pi はデフォルトで RealVNC がはいっている。

ただし、このままだとMac と接続出来ない。

メニューバーでVNC Server の Options を設定する。

![](image-kn8cr7d1.png)



Encryption : Prefer off

Authentication: VNC password

パスワードを設定。

vnc://people.local:5900 でつながる。

### stop したら出なくなった
```shell
$ sudo raspi-config

vnc enable
```

これでメニューに出た。

### 画面からはみ出て設定できん。

メニューから出したウィンドウの上をドラッグして下げたらうまくいった。


