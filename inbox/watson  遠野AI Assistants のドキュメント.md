#watson 


---
2022-03-28  13:24

# watson  遠野AI Assistantsのドキュメント


##  ログインから Assistants へ行く

blue mix コンソール からログイン。
https://cloud.ibm.com/

リソースリストをクリック

![[Pasted image 20220328133141.png]]

Watson Assistant をクリック

![[Pasted image 20220328133405.png]]


Watson Assistant の起動をクリック

![[Pasted image 20220328133553.png]]


## Assistants インスタンス

Assistants インスタンスを作る。これが土台のようなものになる。

![[Pasted image 20220328134001.png]]

Skills インスタンスを作り、Assistants インスタンスとリンクさせる。

![[Pasted image 20220328134321.png]]

## Skills インスタンスの設定
プログラミングするのはこの Skills の中で Intents と Entities と Dialog で設計して設定する。

[[watson  遠野AI のダイアログ設計]]

### Intents の設定
![[Pasted image 20220328134906.png]]

#### \#Chat　雑談をします

User example

```
いい天気ですね、そうですか、それから?、だから、で?、なんで、なんのこっちゃ、なんのことだ、

はい、わかりました、他には、天気いいですね、天気は、天気を教えて、了解
```

#### \#General_About_You　一般的な個人属性を要求します

User example

```
あなたのお年はいくつ、あなたの名前は?、あなたの名前を教えてください、あなたは誰、いくつ、お年は、

お名前は、どなたですか、何について教えてくれますか? 、誰ですか、名前をうかがってもいいですか、

名前を教えてください
```

#### \#General_Agent_Capabilities　ボットの機能を要求します

User example

```
なんて聞けばいい、わからない、わからん、何が出来ますか、何が出来るの、何が答えられますか、

何が答えられるの、言い方がわからない、出来ること教えて
```


#### \#General_Ending　会話を終了します

User example

```
おやすみなさい、お疲れ様です、お返事ありがとうございました、さようなら、さよなら、さいなら、

さようなら、じゃね、どうもありがとう、またね、また今度、また明日、もういいです、以上です、

以上です。失礼します。解決した、完了、今日はこれでokです、終わります、終了します、終わって、

助かったよ、他にありません、売買
```

Speech to Text の漢字変換で「バイバイ」が「売買」なったりするので。



#### \#General_Greetings　ボットに挨拶します

User example

```
あけましておめでとうございます、おはよ、おはよう、おはようございます、こんちは、こんにちは、今日は

こんばんわ、ちょっといいですか?、ちわ、はじめまして、やあ、ヤッホー、教えてください、

今大丈夫?、今よろしいですか、質問してもいいですか、初めまして、
```

#### \#General_Jokes　ジョークを要求します

User example

```
あそびましょ、あそぼー、なんかニュースある?、歌える?、楽しませて、寒くない?、気分はどう?、

私を驚かせて、冗談言ってみて、面白いこと言って、面白いこと言ってください
```


#### \#General_Negative_Feedback　好ましくないフィードバックを表現します

User example

```
これはなんなんだ、なんだこれ、ばか、へんなの、会話にならない、使えない、全く使えない、

役に立たない、役に立ちませんね、役立たず、話しにくい、話にならない
```


#### \#General_Positive_Feedback　肯定的な感情や感謝を表します

User example

```
すごい、すごいです、すばらしいです、ためになった、ためになりました、役立ちました、

よかった、素晴らしい、問題ない、問題なかったです
```


#### \#recommend　おすすめを訪ねます

User example

```
おすすめ、おすすめは何、おすすめを教えて下さい、お勧め、お勧めは
```



### Entities の設定

| Entity  | Value            | Synonyms                                                           |
| ------- | ---------------- | ------------------------------------------------------------------ |
| @crumb  | いい天気ですね   | いい天気、お日柄も良く、天気ですね、天気だ                         |
|         | それから         | それ以外、それで、もっと、後は、他には                             |
|         | はい             | わかった、わかりました、了解しました、了解                         |
| @gender | 男性             | おとこ、男、おのこ                                                 |
|         | 女性             | おんな、女、おみな                                                 |
|         | どちらでもない   | 中性                                                               |
| @joke   | あそびましょ     | あそぼ                                                             |
|         | 歌える?          |                                                                    |
|         | 楽しませて       |                                                                    |
|         | 気分はどう       |                                                                    |
|         | 面白いこと言って |                                                                    |
| @old    | 年齢             | いくつ、ご年齢、お年、年、何歳                                     |
| @reject | 遠慮します       | もう結構、やめろ、結構です、もういい、いらない、入りません、やめて |
| @reply  | そうですか       | へえ、はあ、そう、そうなの                                         |
|         | わかりました     | はい、了解                                                         |
| @thanks | ありがとう       | ありがとね、おおきに、どうも、ありがと                             | 


### Dialog のノード設定

#### ノードの一覧
![[Pasted image 20220328152255.png]]
![[Pasted image 20220328152317.png]]



#### Greetings ノード
![[Pasted image 20220328144744.png]]

#### Introduce Capability ノード

![[Pasted image 20220328144834.png]]

#### About You ノード

![[Pasted image 20220328144907.png]]

#### Recommend ノード(Slots , Multiple condisioned responses)

Slots と Multiple conditioned reponses を ON にした。

![[Pasted image 20220328145052.png]]

Slots の設定

![[Pasted image 20220328145121.png]]

Then check for --> Manage Handlers

![[Pasted image 20220328145323.png]]
![[Pasted image 20220328145348.png]]

Then set context の設定は JSON Editor で出来る。

![[Pasted image 20220328145547.png]]

Assistant reponds は Multiple conditioned reponses にして条件分岐で処理する。

![[Pasted image 20220328145810.png]]
![[Pasted image 20220328145828.png]]
![[Pasted image 20220328145847.png]]

ここで $gender と $age をNULL にしないと保持されたまま、同じ処理を繰り返すので、Response の中でその処理を記述する。

歯車をクリックする。
![[Pasted image 20220328151432.png]]

JSON Editor を Open して、

![[Pasted image 20220328151456.png]]

$gender と $age にnull を入れる設定をする。

![[Pasted image 20220328151531.png]]




#### Chat ノード

![[Pasted image 20220328150016.png]]
![[Pasted image 20220328151054.png]]




#### Joke reply ノード

![[Pasted image 20220328151713.png]]


#### Ending ノード

![[Pasted image 20220328151750.png]]

#### anything_else ノード

どのノードにも引っかからなかったものを引っ掛ける。

![[Pasted image 20220328152033.png]]

この内のどれかをランダムで選ぶ設定をしてる。
![[Pasted image 20220328152048.png]]

