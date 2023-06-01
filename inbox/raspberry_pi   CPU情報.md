#raspberry_pi 


```shell
$ cat /proc/cpuinfo
```

Linux カーネルの bit数を確認

```shell
$ getconf LONG_BIT
32

ガーン知らんかった。
```

cpu は 64bit らしいが、32 bit OS とのこと。

# マシンの情報

```shell
$ uname -m
armv7l


$ uname -m
aarch64
```

Raspberry Pi 4 に armv7l と aarch64 がある!?
どっちも64bit raspberry pi OS でいけるが、SDカードは取っ替えられなかったな。

32 bit OS は安定して動く



