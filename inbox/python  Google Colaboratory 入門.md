#python #colaboratory


[https://qiita.com/shoji9x9/items/0ff0f6f603df18d631ab](https://qiita.com/shoji9x9/items/0ff0f6f603df18d631ab)

## 90分12時間ルール

新しいノートブックを開くと新しいインスタンスが立ち上がる。

ブラウザを閉じると**90分**後にインスタンスが破棄される。

開きっぱなしでも **12時間後**にインスタンスが破棄される。

```bash
あと何分
**$** !cat /proc/uptime | awk '{print $1 /60 /60 /24 "days (" $1 / 60 / 60 "h)"}'
```

[https://colab.research.google.com/notebooks/welcome.ipynb?hl=ja](https://colab.research.google.com/notebooks/welcome.ipynb?hl=ja)

![](image-kn9mdn5g.png)

Colabnotebook とな？ jupyter notebook かな？

新規をチェックしたらその他からの Google Colaboratory で nodebook が作られる。

![](image-kn9me7bq.png)

あとはJupyter notebook と変わらないね。

![](image-kn9mewus.png)