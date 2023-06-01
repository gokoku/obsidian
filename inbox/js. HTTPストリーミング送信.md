---
type: note
---

#js

---
2022-09-02  10:28

# js. HTTPストリーミング送信?

https://twitter.com/nwtgck_ja/status/1564761410571120640


これ何が起こってるのかわからない。

ppng.io  とは?

![[Pasted image 20220902103001.png]]

HTTP を使った Streaming Data 送信サーバ。
https://qiita.com/nwtgck/items/78309fc529da7776cba0

## Piping Server
この人が立ててるサーバだった。


ターミナル1 で受信待ち
```shell
$ curl https://ppng.io/hello
```

ターミナル2 で送信する
```shell
$ echo "Hello World." | curl -T - https://ppng.io/hello
```

すると、ターミナル1 で受信出来る
```shell
$ curl https://ppng.io/hello
Hello World.
```
## この人のツイートをローカルで実行

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <input id="myinput" placeholder="文字を入力して" style="font-size: 1.5em">
  <script>
    const readableStream = new ReadableStream({
      start(ctrl) {
        const encoder = new TextEncoder()
        window.myinput.onkeyup = (e) => {
          if(e.key == 'Enter') {
            ctrl.enqueue(encoder.encode(e.target.value + '\n'))
            e.target.value = ''
          }
        }
      }
    })
    fetch("https://ppng.io/mytexttt", {
      method: 'POST',
      body: readableStream,
      duplex: 'half'
    })
  </script>
</body>
</html>
```

![[Pasted image 20220902111052.png]]

```shell
$ curl https://ppng.io/texttt
上手く行った

Hello
```

凄い、ReadableStream Web API も初耳。

これ、ポーリングしていて、一人と確立したら他の人は掴めない。
pipng.io サーバの仕様? 
マルチキャストのモードもあるらしいが。

## ReadableStream
https://developer.mozilla.org/ja/docs/Web/API/Streams_API/Using_readable_streams

読み取り可能なストリームの使用

ブラウザでサポートしていること前提。
Fetch の Body オブジェクトをストリームとして消費(?)して読み取り可能なストリームを作成する

ReadableStream の start メソッドで enqueue してやると送信されるのか。
Fetch の　body に ReadableStream 、
duplex: 'half' にするのがミソらしい。

TextEncoder がわからん。

## TextEncoder
ここでは文字列をutf 8 バイトコードにする。

a --> 97 = 61(16進)

