#raspberry_pi/osmc 

```bash

$ ssh osmc@osmc.local
passwd : osmc


$ sudo systemctl stop mediacenter   これでディスプレイが止まった

$ rm -rf .kodi/userdata/Thumbnails/*
$ rm .kodi/userdata/Database/MyMusic72.db

$ sudo systemctl start mediacenter    これで元通りになった**
```

この時点ではキャッシュが無いので、Music から入ってもファイルのディレクトリみたいなのを表示して、曲が見えない。


## HDD と NAS の同期

HDD と NAS を同期させるスクリプト作ってた。

```shell
$ bin/rsync_music.sh
```
 
 ```shell
 #!/bin/bash
 
 src=/mnt/nas/iTunes/iTunes\ Media/Music
 dist="/media/music/"
 
 rcync -arv "$src" $dist
 ```

 メニューから Music をクリックするとFiles になる。
 /media/music が HD
 
 /media/music で USBメモリがあったので、music_memoryにrenameした。
 これでこんがらがってたようだ。
なので /media/music_memory が USBメモリ。
満杯になって、rsync_music.sh が途中で終わってたようだった。
 
music を右クリックで Scan item update を選ぶと、右上に読み込んでる曲ファイルが出る。
 
これが終わると、キャッシュに update した /media/music/Music/ がこれで反映されて、一覧ビューになった。
 
 New Albums はその後、時間かかって処理する。


#### Files ソース の生ファイル
~/.kodi/userdata/sources.xml

ここに表示しているファイルリストがある。

```shell
$ sudo systemctl stop mediacenter
$ sudo systemctl start mediacenter
```

Music で Files が出てる時、選んで右クリックして Scan Item to library を選ぶとupdate が始まった。

nas のファイルがあるからここから Scan item to libray で update すれば反映されるのかな。

これで削除したキャッシュが復活するのかな?


 
