#bluemix

---
2021-06-02

# Watson
https://qiita.com/asasaki/items/15e22f359da193669408

https://cloud.ibm.com/

メニュー--> Catalog --> Search for "node-red"

![[Pasted image 20210602134225.png]]


Node RED start application を作る。

![[Pasted image 20210602133847.png]]

こうなった。

![[Pasted image 20210602134038.png]]

instance らしい、free だと一つだけとのこと

ここから Create service で + をクリック -->AI-->Add Watson Assistant

Region を Tokyo にした。

![[Pasted image 20210602141536.png]]

なんかわからんけど、Node RED Bluemix アプリのサービスが
cloudant, Watson Assistant の2つになった。

Watson Assistant から launch --> Assistants -- My FIrst assistant が作られてた。Skills とある。

My first skill をクリックすると、

![[Pasted image 20210602142352.png]]

ごにょごょ

![[Pasted image 20210602142649.png]]

Create intent とはなんぞや。

#bluemix Watson ボット intents 編]]

## Node RED Bluemix App

ダウンロードした。

![[Pasted image 20210602164312.png]]

取り敢えず、node-red をたててみる。
```shell
$ cd node-red-bluemix
$ npm install
$ npm run start
```


動いた。IBM Watson のノードがある。
![[Pasted image 20210602172814.png]]

が、どうやれば動くのだろうか。

ここに Watson Assistant の open dashboard で API key endpoint URL がわかるが、

https://cloud.ibm.com/developer/appservice/apps/9f351756-a203-43a2-8457-73a1bc1f3d35


### text to speech を動かす
https://qiita.com/Kakimoty_Field/items/d6f0524cb620fc26d93d

[Resource list](https://cloud.ibm.com/resources)

ここでCreate resource をクリックする。

Text to Speech をクリック。
![[Pasted image 20210603113945.png]]

Lite プランで Free なのを確認して、Create をクリックする。
![[Pasted image 20210603113128.png]]

ローカルに立てた Node-RED にある text to speech に設定する。
出力用に play audio パレットを install する。

![[Pasted image 20210603114338.png]]

function の中
```js
msg.payload = "はじめまして";
return msg;
```

text to speech の中。 API Key と Endpoint を設定する。

一旦Doneすると、Language と Voice の項目が増えてるので、設定した。

![[Pasted image 20210603114532.png]]


play audio node の中に text to speech 設定後に選べるようになっていた。

![[Pasted image 20210603114658.png]]

しゃべる!!

が Node-RED で Deploy するとエラーが出るようになった。

![[Pasted image 20210603115745.png]]

Lite プランのクエリ上限にひっかかったか?

10,000 Characters per Month とある。

エラーが出るようになったのは、おそらく、Bluemix の App からダウンロードしたコードを使ってて、
Bluemix のダッシュボードで削除したからだろう。

よくわからなくなったので、最初から整理しようと全部削除した。

それで、その設定がしてあったのが壊れたと思われる。

仕切り直しする。

[[bluemix  Watson ボット node-red 編]]