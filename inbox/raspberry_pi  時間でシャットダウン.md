#raspberry_pi 


cron でセットするようだ。

Raspberry Pi を指定時刻にシャットダウン、再起動する – with a Christian Wife [https://blog.withachristianwife.com/2018/07/04/shutdown-pi-on-a-specific-time/](https://blog.withachristianwife.com/2018/07/04/shutdown-pi-on-a-specific-time/)

pi ユーザで crontab -e

19時00分にシャットダウン。

```bash
00 19 * * * sudo /sbin/shutdown -h now
```

9:00 に再起動

```bash
00 9 * * * sudo /sbin/reboot
```

 reboot はシャットダウンした後では効かない。

ユーザのcrontab はどこに書かれるのだろう。
```shell
/var/spool/cron/crontabs/ユーザー名

ex. /var/spool/cro/crontabs/pi
```

このユーザー名のファイルが crontab -e で設定するファイル。

# 効かなかった

同じ設定で効かなかったラズパイがあった。
root で設定したら上手くいった。

```shell
$ sudo su -
# crontab -e
```

```
00 18 * * * /sbin/shutdown -h now
```


