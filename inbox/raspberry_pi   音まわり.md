
#raspberry_pi

## USB Speaker で鳴らす

/boot/config.txt
```shell
# Enable audio (loads snd_bcm2835)
#dtparam=audio=on
```
コメントアウトする。

USB Speaker をつないで、起動する。
このとき、メニューのスピーカーマークが赤バツになっている。
が、鳴る。
確認すると、
```shell
$ cat /proc/asound/modules
1 snd_usb_audio
```

音量はこれで調節できる。
```shell
$ sudo apt-get install alsa-utils sox libsox-fmt-all
$ alsamixer
```
今回使った、USBスピーカは UACDemoV1.0 。
alsamixer で音量上げてやったら鳴るようになった。


![](image-kmq1siml.png)


## HDMI で鳴らす
/boot/config.txt
```shell
# Enable audio (loads snd_bcm2835)
dtparam=audio=on
```

コメント in する。
メニューのすビーカーマークが青になっている。

確認する。

```shell
$ cat /proc/asound/modules
0 snd_bcm2835
1 snd_bcm2835
2 snd_usb_audio
```
## 音チェック


```shell
ビンクノイズ
$ speaker-test -c3

sin波 528Hz
$ speaker-test -t sine -f 528
```

wav ファイル
```shell
$ aplay sound.wav

$ aplay -D plughw:2 sound.wav    # カード番号 2 の時は指定しないと鳴らない
```


# Bullsefe でならなくなった
さっきまで鳴ってたのに、なんだか突然鳴らなくなった。HDMI でもならない。

なんか鳴ってくれ。


### カード番号が変わる
なんかカードやUSBメモリを変えて起動するたびにカード番号が変わる。
USBポートとも関係ない。

```shell
$ aplay -l

**** ハードウェアデバイス PLAYBACK のリスト ****
カード 0: Headphones [bcm2835 Headphones], デバイス 0: bcm2835 Headphones [bcm2835 Headphones]
  サブデバイス: 8/8
  サブデバイス #0: subdevice #0
  サブデバイス #1: subdevice #1
  サブデバイス #2: subdevice #2
  サブデバイス #3: subdevice #3
  サブデバイス #4: subdevice #4
  サブデバイス #5: subdevice #5
  サブデバイス #6: subdevice #6
  サブデバイス #7: subdevice #7
カード 1: vc4hdmi0 [vc4-hdmi-0], デバイス 0: MAI PCM i2s-hifi-0 [MAI PCM i2s-hifi-0]
  サブデバイス: 1/1
  サブデバイス #0: subdevice #0
カード 2: vc4hdmi1 [vc4-hdmi-1], デバイス 0: MAI PCM i2s-hifi-0 [MAI PCM i2s-hifi-0]
  サブデバイス: 1/1
  サブデバイス #0: subdevice #0
カード 3: UACDemoV10 [UACDemoV1.0], デバイス 0: USB Audio [USB Audio]
  サブデバイス: 1/1
  サブデバイス #0: subdevice #0
```

スピーカは UACDemo
カード : 3 デバイス : 0

```shell
$ arecord -l

**** ハードウェアデバイス CAPTURE のリスト ****
カード 4: Device [USB PnP Audio Device], デバイス 0: USB Audio [USB Audio]
  サブデバイス: 1/1
  サブデバイス #0: subdevice #0

```
マイクは USB PnP
カード : 4 デバイス: 0

```shell
$ cat /proc/asound/cards
 0 [Headphones     ]: bcm2835_headpho - bcm2835 Headphones
                      bcm2835 Headphones
 1 [UACDemoV10     ]: USB-Audio - UACDemoV1.0
                      Jieli Technology UACDemoV1.0 at usb-0000:01:00.0-1.3, full speed
 2 [Device         ]: USB-Audio - USB PnP Audio Device
                      USB PnP Audio Device at usb-0000:01:00.0-1.4, full speed
 3 [vc4hdmi0       ]: vc4-hdmi - vc4-hdmi-0
                      vc4-hdmi-0
 4 [vc4hdmi1       ]: vc4-hdmi - vc4-hdmi-1

```

これ一発でわかるか。
これでもわかる。PCM ストリーム調べ。
```shell
$ cat /proc/asound/pcm

00-00: bcm2835 Headphones : bcm2835 Headphones : playback 8
01-00: USB Audio : USB Audio : playback 1
02-00: USB Audio : USB Audio : capture 1
03-00: MAI PCM i2s-hifi-0 : MAI PCM i2s-hifi-0 : playback 1
04-00: MAI PCM i2s-hifi-0 : MAI PCM i2s-hifi-0 : playback 1
```

## 立ち上げるたびにカード番号が変わらないようにする

https://hellobreak.net/raspberry-pi-usb-microphone/

マイクをデフォルトで優先的に認識させるためには

~/.asoundrc を設定するのかな。
とりあえずやってみる。

```shell:~/.asoundrc
pcm.!default {
  type hw
  card 1
  device 0
}
ctl.!default {
  type hw
  card 2
  device 0
}
```
pcm がスピーカ
ctl が録音らしいが、再起動すると .asoundrc は消される。

どーするん?

## 優先順位を変える
/etc/modprobe.d/alsa-base.conf

```config
options snd slots=snd_usb_audio,snd_bcm2835
options snd_usb_audio index=0
options snd_bcm2835 index=1
options snd_usb_audio index=2
options vc4 index=3

```

snd_usb_audio で、mic と speaker が区別できない。
並べて書くと認識しなくなったので、間に snd_bcm2835 (phone ジャック)
を入れた。
ダメ押しで vc4(hdmi) を入れた。

これでなんだかスピーカが card 2  マイクが  card 0 になるようなので、これで行く。



