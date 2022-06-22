#command 

---
2021-09-10

# command ffmpeg で Mov ファイルから mp4 に変換する

```shell
$ ffmpeg -i sample.MOV sample.mp4

QuickTime Player で再生するオプション

$ ffmpeg -i sample.MOV -pix_fmt yuv420p sample.mp4
```

-pix_fmt yuv420p ってなんだろうね。
