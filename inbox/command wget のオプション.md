---
type: note
---

#command 

---
2022-05-19  10:03

# command wget のオプション

-q : quiet 取得状況を表示しない

-r : ウェブサイト丸ごと取得 `wget -r http://example.com`
-l : 階層

 -O file : ファイルに書き出し `wget -O - https://example.com/some.key | sodo apt-key add -`


