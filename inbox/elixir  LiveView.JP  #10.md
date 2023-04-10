---
type: note
---

#elixir

---
2022-09-27  21:13

# elixir  LiveView.JP  #10

https://liveviewjp.connpass.com/event/260861/?utm_campaign=event_reminder&utm_source=notifications&utm_medium=email&utm_content=detail_btn

![[Pasted image 20220927211417.png]]

すごい情報量でした。
疲れて退散。

slack の #liveviewjp チャンネルのノートファイル、うまく動かなかった。
もしかして、最新のやつじゃないと動かんのかな。最新のやつで動いた!!

https://github.com/livebook-dev/livebook

最新のやつ。

elixir 1.14.0 でないと動かない。

```shell
$ git clone https://github.com/livebook-dev/livebook.git
$ cd livebook
$ mix deps.get --only prod

# Run the Livebook server
$ MIX_ENV=prod mix phx.server
```


R の Tidyverse (タイディバース)の dplyr がほしくて Python で pandas を作ったと言ってた。

https://stats.biopapyrus.jp/r/tidyverse/dplyr.html

Elixir で Nx Explorer  のライブラリが dplyr にかなり同等になってるらしい。

https://qiita.com/westbaystars/items/6d70d06540f9ce2e324b


PostgreSQL に接続できるようになったとのこと。

https://qiita.com/torifukukaiou/items/8332fc2bac0778f40d8c



これは livebook の新機能のデモ
https://gist.github.com/piacerex/ca512eca6c83921449ccbc6d62628e40