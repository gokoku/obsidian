---
type: note
---

#osx

---
2022-06-02  15:50

# launchctl で KeePassXC ファイルをNAS にバックアップ

Google Drive において各Mac から KeePassXC のデータファイルを参照している。

KeePassXC データベースファイルがなくなったらアウトである。

お家のMac が NAS にバックアップしてくれるなら安心だなー。

というわけでこのコマンドを定時に実行してもらおう。まあ、8時に起動していたらそのときしてくれ、程度でよい。

NAS  disk1 はユーザログイン項目に入れておく。

![[Pasted image 20220602155303.png | 500]]

```shell
$ /usr/bin/rsync -avh --delete /Users/george/Google\ ドライブ/one /Volumes/disk1
```

これを ~/bin/keepasscx.sh にして bin/にパスを通す。

```bash
#!/bin/bash
/usr/bin/rsync -avh --delete /Users/george/Google\ ドライブ/one /Volumes/disk1
```

```shell
$ chmod +x ~/bin/keepassxc.sh
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
 <dict>
  <key>Label</key>
  <string>george.keepassxc.backup.plist</string>
  <key>ProgramArguments</key>
  <array>
    <string>/Users/george/bin/keepassxc</string>
  </array>
  <key>StartCalendarInterval</key>
  <dict>
    <key>Hour</key>
    <integer>20</integer>
    <key>Minute</key>
    <integer>00</integer>
  </dict>
  
  <key>StandardOutPath</key>
   <string>/tmp/one.log</string> <!--<string>/dev/null</string>-->
  <key>StandardErrorPath</key>
   <string>/tmp/one.error</string> <!--<string>/dev/null</string>-->
 </dict>
</plist>
```

うまくいったら log のところを /dev/null にする。

ラベル名とファイル名を一緒にすると間違いがなくなるかな。

セキュリティで bash を登録する。

![[Pasted image 20220602155734.png | 500]]

