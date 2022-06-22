#vscode #haskell

---
2021-09-30

# Haskell 開発環境

## ツール

```shell
$ stack install stylish-haskell hindent hlint
```

## ghcup をインストールして HLS をインストールする

https://www.haskell.org/ghcup/

```shell
$ curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh

haskell-language-server install に yes した

$ haskell-language-server-8.10.7
```

.zshrc

```shell
# ghcup-env
[ -f "/Users/george/.ghcup/env" ] && source "/Users/george/.ghcup/env"
```

[[haskell   HLS がいいらしい]]

## VSCode 拡張 Haskell をインストールする

https://haskell-language-server.readthedocs.io/en/latest/installation.html

![[Pasted image 20211001094311.png]]

これらも相性いいよと Settings のマークダウンですすめられたのでインストール。

![[Pasted image 20211001105658.png]]

### setting の設定

```json
    "haskell.formattingProvider": "stylish-haskell",
    "haskell.serverExecutablePath": "${HOME}/.ghcup/bin/haskell-language-server-8.10.7",
    "[haskell]": {
        "editor.defaultFormatter": "vigoo.stylish-haskell",
        "editor.formatOnSave": true,
    }

```

こんな感じ。

serverExecutablePath を設定すること。

[haskell] editor で保存時にフォーマットしてくれる!

とりあえずこれで落ち着いた。

