#bluemix #watson #node-red

---
2021-06-03

# Node-RED で Watson Text to Speech を試してみる

https://qiita.com/Kakimoty_Field/items/d6f0524cb620fc26d93d

ここでは、IBM cloud にデプロイした node-red だけど、raspberry pi の node-red でも動かせた。

pallet で node-red-node-watson を install する。

node で text to speech を使う。

![[Pasted image 20210603140417.png]]

function のコード
```js
msg.payload = "どうも";
return msg;
```

text to speech で、username と password  を入れる。

username password は Bluemix のログインアカウント。

ここが、IBM で用意した node-red だと、iam 的なプラグインがセットされていて、入力しなくても良くなってるようだ。

![[Pasted image 20210603140507.png]]

play audio の設定

![[Pasted image 20210603141348.png]]

ちゃんと喋る。

ログインアカウントと、サービス API トークンとエンドポイントURL で動かせる!!

