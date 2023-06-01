#blender 


### blender のレンダリングをPNGにする

![](image-kmyeoyd0.png)

/tmp 以下に連番のpngファイルが出来た。


### この連番で GIF アニメにする

```shell
$ ffmpeg -i %04d.png output.gif
```

