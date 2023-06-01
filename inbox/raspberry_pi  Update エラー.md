#raspberry_pi 

---
2022-03-08  10:51

# raspberry_pi  Update エラー

apt update をしたら、

E: https://packagecloud.io/headmelted/codebuilds/debian/dists/stretch/InRelease の取得に失敗しました  401  Unauthorized [IP: 2600:1f1c:2e5:6901:64f5:fdd1:f007:9802 443]

https://forums.raspberrypi.com/viewtopic.php?t=311367

```shell
$ ls -la /etc/apt/sources.list.d/
-rw-r--r-- 1 root root   71 10月 30  2020 headmelted_vscode.list
-rw-r--r-- 1 root root  187  8月 20  2020 raspi.list
-rw-r--r-- 1 root root   41  3月 26  2021 vscode.list


$ sudo rm -i /etc/apt/sources.list.d/headmelted_vscode.list
$ sudo apt update
$ sudo apt upgrade
```


