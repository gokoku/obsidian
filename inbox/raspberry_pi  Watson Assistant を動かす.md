#raspberry_pi #watson 

---
2021-06-04

# Watson Assistant を動かす

Services でクリエイトする。

Free で。

![[Pasted image 20210604112721.png]]

Launch Watson Assistant をクリックする。
「Watson Assistant の起動」をクリック。
![[Pasted image 20220314164601.png]]

ここから My First assistant をクリック。


![[Pasted image 20210604112824.png]]

ここで色々しこめばいいのかな。

### Create assistant
![[Pasted image 20210604113848.png]]

Add Actions or Dialog skill でスキルを追加する。

![[Pasted image 20210604114507.png]]

Create skill タグで Language を日本語にする。

![[Pasted image 20210604114530.png]]

ところで、スキルとはなんだ?
https://cloud.ibm.com/docs/assistant?topic=assistant-skill-add

- アクションスキル : 会話形フロー、シンプルなインターフェース
- ダイアログスキル : ユーザの質問と要求を理解して答える。自然言語処理



### Create intent

インテントは目的、目標のこと。ユーザからの質問の目的らしい。
intent の recommendations タブがある(有料版のやつか)。
ユーザの質問の recommendations もある。 hide をクリックすると出る。これも有料版のやつだ。

https://cloud.ibm.com/docs/assistant?topic=assistant-intents


![[Pasted image 20210604114626.png]]

intent を create する。

### Create Entities

エンティティーは名詞。
インテントが天気予報の取得のとき、関連する場所と日付のエンティティーが必要。


### Create Dialog

## Node-RED から呼び出し

結局通ったやり方を記す。
assistant の方を使うと出来た。

![[Pasted image 20210607132308.png]]

### assistant の設定

![[Pasted image 20210607132407.png]]

- Username : Bluemix ユーザーアカウント
- Password  : Bluemix パスワード
- API Key                 : Service credentials の API key
- Service Endpoint : Credentials の URL       <--- workspase URL でもない、Assistant ID でもない、
- WorkspaceID        : Skill details の Skill ID   <---注意

Resource not found が帰ってきて困ったが、Endpoint を Watson Assistant のページの Credentials の URL を使うとうまくいった。


https://cloud.ibm.com/resources

Assistants の Dialog のメニューから

![[Pasted image 20210607131722.png]]

View API details を選択する。

![[Pasted image 20210607132111.png]]




![[Pasted image 20210604142129.png]]

Assistant ID は ここの Launch Watson Assistant と、実は endpoint URL もここのやつを使う。








