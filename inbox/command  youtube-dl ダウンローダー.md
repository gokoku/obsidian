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
$ youtube-dl "YouTube動画URL" -F
```


22 が best とある。mp4

```shell
$ youtube-dl "YouTube動画URL" -f 22
```


## youtube-music から

アルバムの再生して URL を copy する。

```
https://music.youtube.com/watch?v=i4p_PO8_OnY&list=OLAK5uy_lrUWXdVyLUnUaD7ohJV0COk3Q8oS73hzk
```

これを youtube-dl に貼る。

```shell
$ youbute-dl "https://music.youtube.com/watch?v=i4p_PO8_OnY&list=OLAK5uy_lrUWXdVyLUnUaD7ohJV0COk3Q8oS73hzk" -F


アルバムの丸ごとならそのままこの URL

$ youbute-dl "https://music.youtube.com/watch?v=i4p_PO8_OnY&list=OLAK5uy_lrUWXdVyLUnUaD7ohJV0COk3Q8oS73hzk" -f 18

かなり時間が掛かる...

3曲目だけなら

[download] Downloading video 3 of 10
[youtube] i4p_PO8_OnY: Dounloding webpage
のパラメータだけでやる

$ youtube-dl "https://music.youtube.com/watch?v=i4p_PO8_OnY -f 18"
```

watch?v=` にパラメータを渡すようだ。

アルバム全体なら

`playlist?list=` 

こんな感じらしい。