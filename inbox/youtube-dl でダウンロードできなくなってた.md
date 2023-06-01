---
type: note
---

#python

---
2023-03-08  19:25

# youtube-dl でダウンロードできなくなってた

brew でinstall してた youtube-dl では出来ないとのことらしい。

```shell
$ youtube-dl "https://youtu.be/PDpZpj2A3F4" -F

ERROR: Unable to extract uploader id; please report this issue on https://yt-dl.org/bug . Make sure you are using the latest version; type  youtube-dl -U  to update. Be sure to call youtube-dl with the --verbose flag and include its complete output.
```

このhttps://yt-dl.org/bug に飛ぶと、https://github.com/ytdl-org/youtube-dl/issues/31794

ytdl-org用ならこれとあった。

```shell
$ brew uninstall youtube-dl

$ pip install git+https://github.com/ytdl-org/youtube-dl.git
```

やってみたら動いた!!

