---
type: note
---

#raspberry_pi 

---
2023-04-04  17:43

# raspberry_pi  apt Upgrade したら Node-REDが動かなくなった

```
  
"ALSA lib pcm_hw.c:1829:(_snd_pcm_hw_open) Invalid value for card↵sox FAIL formats: can't open input `plughw:0,0': snd_pcm_open error: No such file or directory↵"
```

## こうしたら治った

/boot/config.txt
```shell
# Enable audio (loads snd_bcm2835)
#dtparam=audio=on
```
このようにコメントアウトする。

[[raspberry_pi   音まわり]]


## カッパおじさんのやつコメントインしてしまった
```
# Enable audio (loads snd_bcm2835)
dtparam=audio=on
```
けど動いている。てゆーか動かなかったからコメントインして動くようになった。