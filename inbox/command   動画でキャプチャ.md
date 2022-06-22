#command 

---
2021-05-27

# 動画でキャプチャできる

https://dev.classmethod.jp/articles/mac-screen-gif-anime/
Shift + Command + 5 で収録。上部メニューに停止ボタンが現れる。
.mov ファイルが出来る。


# GIF アニメにしたい
```shell
$ brew install ffmpeg

$ ffmpeg -i some.mov some.gif
```