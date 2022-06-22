#raspberry_pi #node-red 

---
2021-06-03

# Node-RED で Watson の Speech to Text を試す

https://qiita.com/egplnt/items/592f6a713dd35770e03c

### cloud IBM の Resource list
https://cloud.ibm.com/resources

Speech to Text と Text to Speech のインスタンスを立てる。

「リソース及びソフトウェア」のところをクリックすると出てくる。

![[Pasted image 20210603175014.png]]

Text to Speech をクリックすると、API と End point URL が取れる。


### Node-RED

![[Pasted image 20210603175147.png]]

テキストから WAV ファイルを生成して、それを読み込んでテキストのデータを取得する形。

#### text to speech の設定
![[Pasted image 20210603175417.png]]

実は ここで Place output on msg.payload にチェックを入れると、
このあとの Function がいらなくて、直接 Speech to Text に繋げられる。

 #### Function のコード
 ```js
msg.payload = msg.speech;
return msg;
```

#### speech to text の設定

一旦設定出来るとこまでしたら完了をクリックする。
その後、もう一度開くとフルで出る。
![[Pasted image 20210603175542.png]]

#### debug でデータの取得

msg.transcription で日本語テキストが取得出来る。

![[Pasted image 20210603175722.png]]

すごい!!

![[Pasted image 20210603175932.png]]

## マイクで拾った声から

マイクで録音する。
watson は 8000Hz じゃ受け付けてくれないとこがわかった。

```shell
$ cd ~/AI/watson
$ arecord -D plughw:2,0 -r 44100 voice.wav
```

この WAV ファイルを speech to text に投げる。
しかも、帰ってきた text を WAV ファイルにして喋らせることが出来た。

![[Pasted image 20210604111104.png]]

#### file in node
![[Pasted image 20210604111141.png]]

Output を a single Buffer object にするとうまく行った。

#### Speech to Text node
![[Pasted image 20210604111236.png]]

何もチェック入れてない。

#### function node

```js
msg.payload = msg.transcription;

return msg;
```

speech to text からは msg.transcription に日本語テキストで帰ってくるので、それを payload に入れて投げる。

#### Text to Speech node

![[Pasted image 20210604111611.png]]

#### play audio node

![[Pasted image 20210604111647.png]]

TTS Voice で 日本語をしゃべる人を選ぶ。

およそ、うまくいく。

![[Pasted image 20210604112211.png]]

