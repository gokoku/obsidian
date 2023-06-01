#chrome_book 

---
2021-07-16

# 素のchromebook からの Linux にする

chrome から Alt + Ctrl + T

```shell
> shell

> sudo mount.sh
> sudo start.sh
```

/usr/local/bin/mount.sh

```shell
#!/bin/sh

mkdir /media/removable/ssd
mount /dev/sda /mediq/removable/ssd
```

/usr/local/bin/start.sh
 
 ```shell
#!/bin/sh

/media/removable/ssd/bin/startxfce4
```

もし、ログアウト後にうまくマウントが出来てなかったときの再マウント

sudo で権限ないと言われたりするとき。


```shell
#!/bin/sh

mount -o remount,symfollow,exec /media/removable/ssd
mount -o remount,symfollow,exec /media/removable/backup
```
