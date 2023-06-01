#watson #ai #node-red

---
2021-07-06

# SLOT を使ってみる

例えば、オススメをするのに、男性か女性か、年齢は、と質問してマトリクスにおとせる。

| 年齢  | 男性     | 女性         |
| ----- | -------- | ------------ |
| 0-9   | アイス   | チョコ       |
| 10-19 | 人参     | アセロラ     |
| 20-29 | キュウリ | ブルーベリー |
| 30-   | 日本酒   | 梅酒         |


## Dialog で slot を選ぶ

![[Pasted image 20210706145718.png]]

Slot と Multiple conditioned responses を ON にする。


## 入力でチェック項目を揃える

チェック項目は、質問項目で、ここでは $gender と $age に代入するものにあたる。

項目を質問する文章を入力する。

![[Pasted image 20210706145934.png]]

@gender は Entyties に登録したもの。

![[Pasted image 20210706151529.png ]]

男とか、おとこ、とかが @gender では「男性」と認識される。

## 条件で出力を変える

Multiple conditioned responses にして、条件で出力を変える。

![[Pasted image 20210706152008.png]]

注意、$age とか変数の前後はスペースを入れること。プログラムと一緒。

あと、一つでもスペースが入ってない箇所があっても、全体で挙動不審になってよくわからなくなるので注意。

![[Pasted image 20210706152333.png]]

これでマトリクスが作れる。

## SLOT をクリアする

![[Pasted image 20210706152506.png]]

終わったらここにジャンプするようにして、ここで変数を null にする。

点々のメニューから JSON で書ける。

![[Pasted image 20210706152827.png]]

"context" の中身が変数。

![[Pasted image 20210706152929.png]]

Jump to で変数クリアノードを選ぶ。

## 途中で辞めたいコース

Then check for の 「Manage handlers」 をクリック。

![[Pasted image 20210706160147.png]]

ここで \#ことわる が入力されたら反応するようにする。

歯車マークから $age と $gender を null にしてクリアする処理を書く。

点々メニューのJSON editer から書いてもいい。

![[Pasted image 20210706160418.png]]

```JSON
{
  "output": {
    "generic": [
      {
        "values": [
          {
            "text": "わかりました。"
          }
        ],
        "response_type": "text"
      }
    ]
  },
  "context": {
    "age": null,
    "gender": null
  }
}
```

それから、この後のフローを設定する。

中断するなら、 Skip to response にする。

![[Pasted image 20210706160542.png]]

このフローは Try it out では response をスキップしないが、API からだとちゃんとスキップするので安心して下さい。
バグですね。

![[Pasted image 20210706160923.png]]
