#raspberry_pi 

---
2022-03-10  11:20

# raspberry_pi  Log の制御をする

一応USBメモリを/var/log にマウントしてある。

ローテーションしたい。

https://www.fabshop.jp/%E3%80%90step-29%E3%80%91raspbian%E3%81%AE%E3%83%AD%E3%82%B0%E5%87%BA%E5%8A%9B%E3%82%92%E6%8A%91%E5%88%B6%E3%81%97%E3%81%A6ssd%E3%82%92%E5%BB%B6%E5%91%BD%E5%8C%96/


```shell

ここに種類がある  --> /etc/rsyslog.conf
RULES 以下に色々


ローテーション設定はここ
$ ls /etc/logrotate.conf

更にこの下にそれぞれ設定されている。
/etc/logrotate.d/



それぞれ
rotate 1
monthly




に設定することにする。(下の設定ファイル)
[alternatives, apt, bootlog, btmp, cups-daemon, dpkg, exim4-base, exim4-paiclog, rsyslog, sane-utils, wtmp]
```


