#command

---
2021-05-27

# 複数の mp3 ファイルを結合する



## 素で書くとこんな感じらしい

https://yatta47.hateblo.jp/entry/2016/07/19/200000

```shell
$ ffmpeg -i input1.mp4 -i input2.mp4 -filter\_complex "concat=n=2:v=1:a=1" output.mp4
```

-filter_complex "concat=n=2:v=1:a=1"
n=動画数
v=1 (ビデオ 1:結合 0:結合しない)
a=1 (オーディオ 1:結合 0:結合しない)

めんどくさい。

## 入力リストを作って指定できる

```shell
$ ffmpeg -f concat -i mylist.txt -c copy output

$ ffmpeg -f concat -safe 0 -i mylist.txt -c copy output.mp3  # -safe 0 は Unsafe file name と出るとき用
```

mylist.txt
```
file '/path/to/file1'
file '/path/to/file2'
...
```

## やってみる
https://qiita.com/ruri14/items/8556bf3b7f86ed659be8

```shell
$ cd path
$ for f in *.mp3; do echo "file '$f'" >> mylist.txt; done
$ ffmpeg -f concat -i mylist.txt -c copy output.mp3
```

---
## カバーアートも付けたくなった

アートワークを抽出することができる。
```shell
$ ffmpeg -i audio.mp3 artwork.jpg
```

https://stackoverflow.com/questions/18710992/how-to-add-album-art-with-ffmpeg/48829530
この通り打ち込んでみる。

```shell
 ffmpeg -i Unit_1.mp3 -i artwork.jpg -map 0:0 -map 1:0 -c copy -id3v2_version 3 -metadata:s:v title="Album cover" -metadata:s:v comment="Cover (front)" out.mp3
```

![[Pasted image 20210527230303.png]]

ffprobe という python ツールでメタデータがみれるらしい。
つまり、アーティスト、アルバム名といれられれば、iTunes に追加すれば自動的にまとまってくれないだろかと。





