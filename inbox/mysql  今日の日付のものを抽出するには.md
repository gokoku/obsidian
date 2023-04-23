---
type: note
---

#mysql 

---
2023-04-21  14:06

# mysql  今日の日付のものを抽出するには

今日の日付を知る関数

```mysql
select curdate();
```

これで date フィールドの中が今日のものをとって来るには

```mysql
select * from calendar_dates where date = curdate();
```

chatGPT に教えてもらいました。
すごくね?!

