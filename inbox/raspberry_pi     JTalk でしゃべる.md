#raspberry_pi 

```bash
$ sudo apt-get install -y open-jtalk open-jtalk-mecab-naist-jdic hts-voice-nitech-jp-atr503-m001
```

jtalk.sh

```bash
#!/bin/bash
tempfile=`tempfile`
option="-m /usr/share/hts-voice/nitech-jp-atr503-m001/nitech_jp_atr503_m001.htsvoice \
-x /var/lib/mecab/dic/open-jtalk/naist-jdic \
-ow $tempfile"
echo "$1" | open_jtalk $option
aplay -q $tempfile
rm $tempfile
```

```bash
$ chmod +x jtalk.sh

$ ./jtalk.sh "こんにちは"
```

## 声質を変えてみる

-r : スピーチ速度係数 0~1.0



## 声をおっさんからメイちゃんへ

```bash
$ wget https://sourceforge.net/projects/mmdagent/files/MMDAgent_Example/MMDAgent_Example-1.7/MMDAgent_Example-1.7.zip --no-check-certificate

$ unzip MMDAgent_Example-1.7.zip

$ sudo cp -R ./MMDAgent_example-1.7/Voice/mei /usr/share/hts-voice
```

## node-red で喋らせる

![](image-kn8ak4ar.png)

![](image-kn8akc2h.png)

これで先に作った shell script を走らせる。

![](image-kn8als49.png)

