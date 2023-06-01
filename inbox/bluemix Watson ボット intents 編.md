#bluemix #watson #ai

---
2021-06-02

# Watson 使ってみる

https://jp-tok.assistant.watson.cloud.ibm.com/crn%3Av1%3Abluemix%3Apublic%3Aconversation%3Ajp-tok%3Aa%2F59b3f141352749939b09dd73209ddf01%3A04ed1851-fafb-4b99-b70b-59a47e030b22%3A%3A/skills


My first skill という何かが作られたところから。

![[Pasted image 20210602144926.png]]

## intents を作る

これは質問集だ。

General_About_Youという元々入っている intent を+してみた。

![[Pasted image 20210602181554.png]]

こんな感じに書くのか。
- intent って質問のくくりだね。
- description がその説明だね。
- その中で、それに関係する質問をずらずらーと入れてくのがわかるが、 User example



これに習って、あいさつをを登録する。 User example で書く。

![[Pasted image 20210602182309.png]]

右上の Try it で動いてるのが見られる。

![[Pasted image 20210602145224.png]]

下のフォームに打ち込むと、なんと判別するのかがわかる。














## Entities を作る

対話の目的語らしいが、名詞のことかな?

![[Pasted image 20210602145915.png]]

@vegetables で、いっぱい単語を登録してみる。

Synonyms ってなんだ? 同義語か。言葉違いでこれと同じものを書くらしい。

## Dialog で対応づける

![[Pasted image 20210602151748.png]]

If assistant recognizes で反応して、responds をかえすようだ。

![[Pasted image 20210602161357.png]]

node にぶら下がる子が付けられるので、会話似するトトが出来る。

## 他たくさん

また後で。
