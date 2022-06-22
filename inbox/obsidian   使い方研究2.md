#obsidian

---
2022-04-24  21:45

# obsidian   使い方研究2
## Front matter を使ってみる?

[[obsidian Obsidian 使い方研究3]]

```
---
aliases: hoge1, hoge2
tags: js, ts
---
```

公式ではこの4つとのこと。

- alias は検索に揺れを入れられるようにする。
- tags はプレビューに現れないタグ名、pdf にしたときに印刷されないタグ名
- csssclass : このページ用に CSS クラスを指定出来るとのこと。
- publich : Valt を公開するかしないか false true

あとは、自由。

type とか
その場合は、 dataview を使って抽出するようだ。
まるで DB のように使う。

```
---
tags: person
age: 28
---
```

from タグ名
```
\```dataview
table age as 年齢 from #person
where age = "28"  とか where contains(summary, "Alice")
sort age desc
\```
```

## Dataview

TABLE
LIST
TASK
CALENDAR

Front matter でなくてもこう書くと dataview で抽出してくれる。

```
---
type: person
---
```
とか
```
type:: person
location:: Singapore
```
とすると、
```
\```dataview
LIST location
WHERE type = "person"
\```
```

### Tables in Dataview

```
---
Title: Dune
Author: Frank Herbert
Tags: TVZ, readwise, books
date: 2022-03-27
rating: 5
---


\```dataview
TABLE rating as "Rating" from #books
WHERE date >= date(2022-01-01) and date < date(2023-01-01)
sort date asc
\```
```


### Tasks in Dataview

```
\```dataview
TASK
WHERE project = "Project X" and !completed
\```
```

### Calendar in Dataview

```dataview
CALENDAR file.mtime from #todo
```










