---
type: note
---

#notion

---
2022-04-26  14:42

# Notion でプロジェクト管理をしてみる


<div class="rich-link-card-container"><a class="rich-link-card" href="https://news.mynavi.jp/techplus/article/howtonotion-9/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://news.mynavi.jp/techplus/article/howtonotion-9/ogp_images/ogp.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Notionでプロジェクト管理をしよう - Notionがあなたのチームを強くする(9)</h1>
		<p class="rich-link-card-description">
		今話題のNotionというツールをご存知でしょうか。Wiki、ドキュメント管理、プロジェクト管理といったチームに必要な情報をすべて1つに集約できるツールです。Notionを使ってあなたのチームをさらに強くする方法を解説します。
		</p>
		<p class="rich-link-href">
		https://news.mynavi.jp/techplus/article/howtonotion-9/
		</p>
	</div>
</a></div>


これで学んだ成果まとめ。

## ページとデータベースを作る

ぴーぷるWork(page) の中に、
- Projects(database)  : プロジェクト
- Tasks(database)      : 作業タスク
- Documents(database) : 自分ドキュメント、資料、pdf など (myself, outside, )
- Minutes(database)      : 議事録
- Notices(database)     : 気づいたことメモ、日記? 感想?

![[Pasted image 20220426145454.png]]

### Project properties

**Layout --> Gallery**
- Card preview --> Page cover
- Card size --> Medium
- Fit image --> false

**Property**

| Name                   | Type                           | Hide |
| ---------------------- | ------------------------------ | ---- |
| Project Name           | Title                          | show |
| Staff                  | Person                         | show |
| Status                 | Select(Todo, InProgress, Done) | show |
| Due Date               | Date                           | show |
| Total Hours            | RollUp (後で設定する)          | show |
|                        |                                |      |
| Related to Tasks     | Relation --> Tasks             | hide |
| Related  to Documents | Relation --> Documents         | hide |
| Related to Minutes    | Relation --> minutes           | hide |


### Tasks properties

**Layout --> Table**
- Wrap all columns --> true


**Properties**

| Name                | Type                           | Hide |
| ------------------- | ------------------------------ | ---- |
| Task Name           | Title                          | show |
| Staff               | Person                         | show |
| Status              | Select(Todo, InProgress, Done) | show |
| Due Date            | Date                           | show |
| Total Hours         | Number                         | show |
| Related to Projects | Relation --> Projects          | show |



**Sort**
Due Date -- Ascending --> Save for everyone

**Group by**

|                   |                                |
| ----------------- | ------------------------------ |
| Group by          | Related to Projects            |
| Sort              | Alphabetical(アルファベット順) |
| Hide empty proups | flase                          |
|                   |                                |


### Documents properties 手作り資料等
**Layout --> Table**
- Wrap all columns --> true

**Properties**

| Name               | Type                       | Hide |
| ------------------ | -------------------------- | ---- |
| Document Name      | Title                      | show |
| Tags               | Multi-Select               | show |
| Types              | Select(MyDocument, Others) | show |
| Date               | Date                       | show |
| Related to Project | Relation --> Projects      | show |



### Minutes properties 議事録
**Layout --> Table**
- Wrap all columns --> true

**Properties**

| Name               | Type                  | Hide |
| ------------------ | --------------------- | ---- |
| Meeting Name       | Title                 | show |
| Members            | Multi-Select          | show |
| Date               | Date                  | show |
| Related to Project | Relation --> Projects | show |



### Noticed properties

**Layout --> Table**
- Wrap all columns --> true

**Properties**

| Name   | Type                    | Hide |
| ------ | ----------------------- | ---- |
| Title  | Title                   | show |
| Date   | Date                    | show |
| Status | Select(PickUp, Pending) | show |
|        |                         |      |



## Project Template を作る


![[Pasted image 20220426170203.png]]
New から New template で追加する。

#### Properties を整える

![[Pasted image 20220426170340.png | 500]]

Title --> Project Template (テンプレート名)

Properties を順番に並べて、リレーション系は非表示にする。

非表示は、Hide property --> Always hide をチェックする。
![[Pasted image 20220426170956.png | 400]]

#### コンテンツの作り込み

##### Tasks の Board View の追加
![[Pasted image 20220426172054.png]]
**Board**
ブロックでデータベースの Board を選んで Tasks を選択する。

**Properties**
Task Name , Staff だけ show にした。

**Filter**
Related to Projects を選んで、このページのタイトルを選択する。
![[Pasted image 20220426173604.png]]
これだけで、各ページで展開されるテンプレートにこのプロジェクト関連のタスクだけに絞られるのですごい。

save for everyone をクリツクすること。

![[Pasted image 20220426173921.png | 500]]

##### Task の Timeline View を追加

![[Pasted image 20220426174203.png | 500]]
Table properties から テーブルとTimeline の表示プロパティを調整出来る。

Timeline に Staff を付けた。
![[Pasted image 20220426174634.png | 300]]

出来たところ。


![[Pasted image 20220426174740.png | 600]]




##### Minutes 議事録のView を追加する

Table
![[Pasted image 20220426181315.png | 500]]
Properties はリレーションだけ Hide にする。
![[Pasted image 20220426181440.png | 200]]


##### Documents の View を付ける

Minutes と同じようにした。

以上でテンプレートを作ったので、プロジェクトを立てるときはこれで立てるといいのだ。

![[Pasted image 20220426181838.png]]
![[Pasted image 20220426181858.png]]


##### Template の編集
![[Pasted image 20220426182031.png]]

ここから Edit で入る。入ったら Back で抜ける。

各ページで Template が用意できるので、Project の template は Project を開いてから New する。

## ぴーぷるWorks( トップページ ) のレイアウトを作る

![[Pasted image 20220426182444.png]]

こんな感じで、気づいた事 Notions の View を付けた。
改善メモなのでどこにもリレーションしなくていいので。

後は、データベースたちは inner にしたり、フルページにしたり出来るけど、
邪魔だからと行って削除したらデータベースごと無くなるので注意。

