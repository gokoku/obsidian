#command 

---
2021-06-03

# マイクが busy で使えないとき

コマンドで録音しようとして、
```shell
$ arecord -D plughw:2,0 test.wav

device is busy
```
使えない。


デバイスがマウントされっぱなしなようだ。

```shell
$ lsof /dev

arecord   3215   pi  mem    CHR 116,88           408 /dev/snd/pcmC2D0c
arecord   3215   pi    0r   CHR    1,3      0t0    6 /dev/null
arecord   3215   pi    2u   CHR    1,3      0t0    6 /dev/null
arecord   3215   pi    4u   CHR 116,88      0t0  408 /dev/snd/pcmC2D0c
```

これを kill すれば、使えるようになる。

```shell
$ kill 3215
$ arecord -D plughw:2,0 test.wav
OK
```

