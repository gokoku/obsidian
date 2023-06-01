#raspberry_pi 


# データバックアップ

この下にあるスクリプトを標準USBメモリデータのバックアップにすることにしよう。


ホームの/home/pi 丸ごとディレクトリと、
素材リソース /media/pi/MEDIA の丸ごとディレクトリだけをバックアップすればソースからデータを確保出来るはず。

## SDカードの名前を付ける

backup という名前にして、差し込むと名前の特定されたスクリプトでバックアップを取れるようにしたい。

```shell
$ sudo lsblk 
sdc           8:32   1 28.9G  0 disk
└─sdc1        8:33   1 28.9G  0 part /media/pi/3640276e-7cca-4558-8ad4-65273a3b1cd3
```

デバイス名 /dev/sdc1 とわかった。
確認。タイプを調べられるオプションつけてみた。

```shell
$ df -T

/dev/sdc1      ext4        29645804 2202268 25914552    8% /media/pi/3640276e-7cca-4558-8ad4-65273a3b1cd3
```

デバイスラベルの変更。
```shell
$ sudo e2label /dev/sdc1 backup
```

これで umount mount すると backup となった。

## バックアップ用スクリプト

特定の場所のバックアップをするだけにするので、シンプルにまるっと削除。
まるっとディレクトリコピーするだけにする。

/home/pi/bin/backup.sh

```shell
#!/bin/bash

src1=/home/pi

dist=/media/pi/backup/

echo "$src1 --> $dist を同期します。"
rsync -arv --delete $src1 $dist

echo "****** 終了しました *********"
```

## やり方

backup という SDカードを USB でさす。
```shell
$ backup.sh
```
アンマウントする。
```shell
$ sudo umount /media/pi/backup
```
マウント先のディレクトリ名でアンマウントする。


## システム SD カードのバックアップ

#raspberry_pi    SDカードのバックアップ backup