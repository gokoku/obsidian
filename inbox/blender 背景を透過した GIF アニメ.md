#blender 


![](image-kmyezl3a.png)

Film  —> Tranpearent チェック

![](image-kmyf00oh.png)

FFmpeg では透過できる形式が限られている。

![](image-kmyf0vd6.png)

Alpha チャンネルがあるか確認すればいいようだ。

---
## 透過GIFアニメにする

PNG で連番出力したものを FFmpeg で連結する。

```shell
$ ffmpeg -i %04d.png -filter_complex "fps=24, setpts=PTS/0.5, split[a][b];[a] palettegen [p];[b][p] paletteuse=dither=none" -y output.gif
```

![](image-kmyf2oni.png)

透過GIFアニメが出来る。
