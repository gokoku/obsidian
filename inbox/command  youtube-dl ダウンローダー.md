#command

---
2021-08-09

# youtube-dl ダウンローダー

  
```shell
$ brew install youtube-dl
```
  

ダウンロード出来る利用可能な形式の一覧を見る。

URL は右クリツクで選択する。ブラウザのURLとは違うので注意。


https://youtube.com/watch?v=oDZiYcSegA 　とあったら、

https://youtu.be/oDZiYcSegA   が動画URLのようだ。


```shell
$ youtube-dl YouTube動画URL -F
```
  

22 が best とある。mp4

```shell
$ youtube-dl YouTube動画URL -f 22
```
