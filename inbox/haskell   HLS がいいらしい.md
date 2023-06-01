#haskell

---
2021-09-30

# HLS haskell-language-server

HIE haskell-ide-engine は開発終了で、いまいちだったらしい。

HIE + ghcide = HLS らしい。

https://zenn.dev/konn/articles/1a60baba9848a1

というわけで、HLS の VScode 版を入れることにした。

![[Pasted image 20210930172230.png]]

https://www.haskell.org/ghcup/

これを入れてくれとのこと、自動アップデートするのかな?

```shell
$ curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh

HLS はインストール
stack はインストールしない
パスをzshにつける。なかったから失敗。 

Press ENTER 
```

zsh/.zshrc に追加する

```shell

# ghcup-env
[ -f "/Users/george/.ghcup/env" ] && source "/Users/george/.ghcup/env"

```

.cabel .ghc .ghcup が出来てる。

まあこれでいいようだ。

