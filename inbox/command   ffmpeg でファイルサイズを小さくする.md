#command 

---
2021-05-27

# ffmpeg でファイルサイズを小さくする

https://mankindinc.jp/2018/03/14/ffmpegの動画圧縮・変換コマンドの使い方/
圧縮率 -crf  高いほど高圧縮

```shell
$ ffmpeg -i example_original.mp4 -crf 32 example_compressed.mp4
```

32 くらいだとスマフォ動画にちょうどいいらしい。
リサイズ
幅だけ指定してアスペクト比を計算してくれるオプション -vf scale=300:-1

```shell
$ ffmpeg -i sample.mp4 -vf scale=300:-1 sample_resize.mp4
```


透過アニメをそのまま透過にしたい。

```shell
$ ffmpeg -i chibi_fuka.gif -filter_complex "[0:v] scale=300:-1:flags=lanczos,split [a][b]; [a] palettegen=reserve_transparent=on:transparency_color=ffffff [p]; [b][p] paletteuse" comp/chibi_fuka.gif
```

これで出来た
