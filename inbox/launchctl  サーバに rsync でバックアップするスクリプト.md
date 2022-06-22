---
type: note
---

#osx #command

---
2022-06-02  16:00

# launchctl  サーバに rsync でバックアップするスクリプト

`bin / rsync_works`

```shell
#!/bin/bash
#   work_progress と work_closed を peopleサーバで同期させる
#   vhostsも同期

my_src1=$HOME/Documents/work_progress/
my_remote1='people2:/Volumes/Members_HDD/tada/work_progress'

my_src2=$HOME/Documents/work_closed/
my_remote2='people2:/Volumes/Members_HDD/tada/work_closed'

my_src3=$HOME/Documents/work_archives/
my_remote3='people2:/Volumes/Members_HDD/tada/work_archives'

#my_src4=/var/www/vhosts/
#my_remote4='people2:/Volumes/Members_HDD/tada/vhosts'

my_src5=$HOME/bin/
my_remote5='people2:/Volumes/Members_HDD/tada/bin'

# 同期する
echo "同期1：$my_src1 --> $my_remote1"
rsync -arv --delete $my_src1 $my_remote1

# 同期する
echo "同期2：$my_src2 --> $my_remote2"
rsync -arv --delete $my_src2 $my_remote2

# 貯める
echo "同期3：$my_src3 --> $my_remote3"
rsync -arv $my_src3 $my_remote3

# 貯める
#echo "同期4：$my_src4 --> $my_remote4"
#rsync -arv $my_src4 $my_remote4

# 同期する
echo "同期5：$my_src5 --> $my_remote5"
rsync -arv --delete $my_src5 $my_remote5
```

しかし、パスフレーズを訊かれて止まる。

## パスフレーズなしでログインできるようにする

パスフレーズ無しの鍵ペアを作って、公開鍵を people2 の authorized_keys に追記する。

```shell
$ ssh-keygen -t rsa
/Users/george/.ssh/id_rsa_np
リターン
リターン
```

id_rsa_np というノーパスフレーズ鍵ペアを作ったので、id_rsa_np.pub を people2 に設置する。

```shell
$ scp ~/.ssh/id_rsa_np.pub people2:~/.ssh/
```

config で people2 に入るところの証明書を id_rsa_np にする。

```shell
Host poeople2
  Hostname 192.168.20.16
  User peoplr2
  IdentityFile ~/.ssh/id_rsa_np
```


##### サーバ側
authrized_keys : 600

```shell
$ cat id_rsa_np.pub >> authorized_keys
```

##### クライアント側
```shell
$ ssh people2
>
```

OK!!

### Launchctl をで定時起動させる

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>rsync.works.backup</string>
  <key>ProgramArguments</key>
  <array>
    <string>/Users/george/bin/rsync_works</string>
  </array>
  <key>StartCalendarInterval</key>
  <dict>
    <key>Hour</key>
    <integer>18</integer>
    <key>Minute</key>
    <integer>07</integer>
  </dict>
  <key>StandardErrorPath</key>
  <string>/tmp/rsync_works.err</string>
  <key>StandardOutPath</key>
  <string>/tmp/rsync_works.log</string>
</dict>
</plist>
```

error が出て動かなかった。

```
rsync: opendir "/Users/george/Documents/work_progress/." failed: Operation not permitted (1)
rsync error: some files could not be transferred (code 23) at /AppleInternal/Library/BuildRoots...
```

環境設定-->セキュリティとプライバシー-->フルディスクアクセス

`/bin/bash` を許可に加えた。

![[Pasted image 20220603181441.png | 400]]

上手くいった!!