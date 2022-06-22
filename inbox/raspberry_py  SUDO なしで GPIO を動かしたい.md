#raspberry_pi  #python 

---
2022-01-04

# GPIO without sudo

python で WS2815 LED を動かすのに sudo が必要だ。

外から動かしたいのだけど、どーする?

https://raspberrypi.stackexchange.com/questions/40105/access-gpio-pins-without-root-no-access-to-dev-mem-try-running-as-root


### gpio グループ

gpio group のメンバーになる。

```shell
$ sudo adduser pi gpio
```

グループの確認。

```shell
$ cat /etx/group | grep gpio
```

### gpiomem のパーミッション

```shell
$ ls -l /dev/gpiomem
crw-rw---- 1 root gpio 244, 0 Dec 28 22:51 /dev/gpiomem


$ sudo chown root.gpio /dev/gpiomem
$ sudo chmod g+rw /dev/gpiomem
```
変わらなかった。


Node-RED で動かしたかっただけで、sudo 付きで exec 出来たのでまあいっか。

