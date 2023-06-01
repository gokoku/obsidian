#js/p5 



# ready

p5 マネージャーを便利に使おう。

```shell
$ yarn global add p5-manager

$ p5 g -b speech
$ cd speech
```

\-b バンドルはライブラリも一括で生成するようだ。

p5.speech.js をインストールしたい。

<https://idmnyu.github.io/p5.js-speech/>

ダウンロードしてファイルをlibrariesに設置する。

libraries/p5.speech.js

index.html に追記

```html
	<script src="libraries/p5.speech.js"></script>
```

* * *

sketch.js

```js
let voice, input, speakButton

function setup() {
  createCanvas(windowWidth, windowHeight)

  input = createInput('こんにちは')
  input.position(20, 100)
  speakButton = createButton('Speak')
  speakButton.position(190, 100)
  speakButton.mousePressed(doSpeak)
  voice = new Voice()

  voice.speak(
    'どうもはじめまして。わたしは誰でしょう? そんなことはさておき、テキストを入力してください'
  )
}

function draw() {}

function doSpeak() {
  voice.speak(input.value())
}
```

voice.js

```js
class Voice {
  voice = new p5.Speech()

  constructor() {
    this.voice.setLang('ja')
    //this.voice.setVoice('Google 粤語（香港）')
    this.voice.setVoice('Google 日本語')
    //this.voice.setVoice('Kyoko')
    this.voice.setRate(1.3)
    this.voice.setPitch(1)
    //console.log(voice.listVoices())
  }

  speak(str) {
    this.voice.speak(str)
  }
}
```

```html
	<script src="voice.js"></script>
	<script src="sketch.js"></script>
```

単純に class にして分割してみた。

Vuejs と合体するやり方とかやる人はいるが、どうも無駄に複雑になる。


