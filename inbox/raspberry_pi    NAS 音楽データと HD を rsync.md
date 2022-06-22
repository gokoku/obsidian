#raspberry_pi/osmc 

パスにスペースが入ってたので、いろいろやってこうなった。

~/bin/rsync_music.sh

```bash
#!/bin/bash

src=/mnt/nas/iTunes/iTunes\ Media/Music
dist="/media/music/Music/"

rsync -arv "$src" $dist
```

```bash
$ chmod +x rsync_music.sh
```

これを cron で回そう。

```bash
0 20 * * * /home/pi/bin/rsync_music.sh 1>/dev/null 2>/dev/null
```

毎日8時についてたら実行ということで。

---

OSMC で cron が not found

My OSMC のカートアイコンでインストール出来る。App Store だ。

Cron Task Scheduler

$ sudo service cron status 

active(running)

再起動したら crontab が使えるようになった。

```bash
0 21 * * * /home/osmc/bin/rsync_music.sh 1>/dev/null 2>/dev/null
```

rsync がない。

```bash
$ sudo apt install rsync
```

cron が動いてくれない。