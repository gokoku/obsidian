#raspberry_pi 

```bash
調べる。
これが現在の解像度。

$ tvservice -s
```

設定する

/boot/config.txt を編集する。

```bash
念の為コピー
$ sudo cp -p /boot/config.txt /boot/config.txt.org 

hdmi_group=1     # CEA
hdmi_mode=1      # VGA

これを変更するとのこと。

```

ここに値の対応表がある。

[https://www.raspberrypi.org/documentation/configuration/config-txt/video.md](https://www.raspberrypi.org/documentation/configuration/config-txt/video.md)

会社の小さなモニターが映らなくなったのでなんとたしてみた。

モニターのメニューから16:9 とわかった。60Hz

これを raspberry pi の config.txt に設定してみたらうまく行った

```bash
hdmi_group=1      # CEA
hdmi_mode=5      # 1080p 16:9 60Hz
```

ついてくれるようになったが、mode =3 でも 4 でもOK

#### デフォルトでhdmiディスプレイにする

ディスプレイが繋がってないとVNC で繋がってもデスクトップ画面がでない。

/boot/config.txt

```shell
hdmi_force_hotplug=1
hdmi_group=1
hdmi_mode=1
```

コメントを外す。

reboot。うまくいった。ディスプレイが繋がってなくても、VNC でディスプレイが見える。


---

また、出なくなった。

overscan を off にして、なおかつ mode を 3 にして、audio をoff にしたら映るようになった。

```jsx
disable_overscan=1

hdmi_mode=3
```

なんで??