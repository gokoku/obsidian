#raspberry_pi 


```shell
/etc/rsyslog.conf の設定

cron.*        /var/log/cron.log     <--コメントイン
```

```shell
$ sudo /etc/init.d/rsyslog restart
```
