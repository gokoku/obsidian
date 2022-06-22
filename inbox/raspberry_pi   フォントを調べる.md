#raspberry_pi 

使えるフォントを調べる。

```bash
$ fc-list
```

---

python のライブラリでフォントを指定した。

```bash
$ fc-list

...
/usr/share/fonts/opentype/noto/NotoSerifCJK-Bold.ttc: Noto Serif CJK SC:style=Bold
...
```

```bash
self._font = ImageFont.truetype("NotoSansCJK-Bold", 32)
```

sans はゴシック?、serif は明朝?

