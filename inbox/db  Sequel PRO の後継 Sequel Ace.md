#db

---
2022-01-26  21:48

# db Sequel PRO の後継 Sequel Ace

<div class="rich-link-card-container"><a class="rich-link-card" href="https://rooter.jp/programming/how-to-use-sequel-ace/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://rooter.jp/wp-content/uploads/2021/08/how-to-use-sequel-ace_-1-1.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Mac用MySQL GUIクライアントアプリSequel Aceの基本操作（Sequel Pro後継）</h1>
		<p class="rich-link-card-description">
		はじめに はじめまして、株式会社rooterで学生バイトをしている友藤です。普段の業務でDBを操作するのにSequel Aceを利用しています。今回は、そのSequel Aceの便利な機能について書い
		</p>
		<p class="rich-link-href">
		https://rooter.jp/programming/how-to-use-sequel-ace/
		</p>
	</div>
</a></div>

## インストール

![[Pasted image 20220126215451.png]]

あっDBがない。

[[docker  MySQL5.7 を立てる]]

## 接続する

![[Pasted image 20220126223634.png]]

普通に接続出来た。

## やりたかったこと

### 問題
応用情報試験問題の SQL を試して見たかったのだ。

| 部門コード | 第1期売上 | 第2期売上 |
| ---------- | --------- | --------- |
| D01        | 1,000     | 4,000     |
| D02        | 2,000     | 5,000     |
| D03        | 3,000     | 8,000     |


このデータベースからこの結果を出すには?

| 部門コード | 期    | 売上  |
| ---------- | ----- | ----- |
| D01        | 第1期 | 1,000 |
| D01        | 第2期 | 4,000 |
| D02        | 第1期 | 2,000 |
| D02        | 第2期 | 5,000 |
| D03        | 第1期 | 3,000 |
| D03        | 第2期 | 8,000 |

### Sequel Ace で実習

![[Pasted image 20220126224923.png|600]]

```sql
select 部門コード, '第1期' as 期, 第1期売上 as 売上 from 売上問題;
```

この結果!
![[Pasted image 20220126225353.png| 300]]

```sql
select 部門コード, '第2期' as 期, 第2期売上 as 売上 from 売上問題;
```

この結果!!
![[Pasted image 20220126225529.png|300]]


この二つのOR集合を取ればいいとのこと。
和集合は UNION とのこと。

```sql
select 部門コード, '第1期' as 期, 第1期売上 as 売上 from 売上問題
union
(select 部門コード, '第2期' as 期, 第2期売上 as 売上 from 売上問題);
```
![[Pasted image 20220126225923.png|300]]

後はこのソートをすればいいのか。

```sql
select 部門コード, '第1期' as 期, 第1期売上 as 売上 from 売上問題
union
(select 部門コード, '第2期' as 期, 第2期売上 as 売上 from 売上問題)
order by 部門コード, 期;
```

出来た！
![[Pasted image 20220126230129.png|300]]

これがやりたかった!!
問題の実践。楽しいー!!