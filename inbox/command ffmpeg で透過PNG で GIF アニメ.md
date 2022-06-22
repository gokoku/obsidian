#command 

---
2021-05-27

# ffmpeg で透過PNG で GIF アニメを作る

```shell
$ ffmpeg -i %04d.png -filter_complex "fps=24, setpts=PTS/0.5, split[a][b];[a] palettegen [p];[b][p] paletteuse=dither=none" -y output.gif
```

png は連番。blender の出力。
スピードは stepts で調整する。

https://trac.ffmpeg.org/wiki/How to speed up / slow down a video
fps は滑らかさ。
ファイル名は  001.png 002.png 003.png....
```shell
$ ffmpeg -i %03d.png -filter_complex "fps=24, setpts=PTS*3.0, split[a][b];[a] palettegen [p];[b][p] paletteuse=dither=none" -y output.gif
```

```shell
$ ffmpeg -i %03d.png -filter_complex "fps=24, setpts=PTS*4.0, split[a][b];[a] palettegen [p];[b][p] paletteuse=dither=none" -y output.gif
```

```shell
$ ffmpeg -i %03d.png -filter_complex "fps=48, setpts=PTS*5.0, split[a][b];[a] palettegen [p];[b][p] paletteuse=dither=none" -y output.gif
```
