---
type: note
---

#elixir 

---
2023-02-05  21:09

# elixir Livebook Desktop のホームディレクトリを指定する


Mac の livebook desktop のホームディレクトリが $HOME なのをなんとかしたい。

```shell
export HOME="$HOME/Google Drive/マイドライブ/livebook/"
```

これを
/Applications/Livebook.app/Contents/Resources/rel/bin/app
に直に書き込むと上手くこのディレクトリがhomeになった。

.zshrc の環境変数として設定しても効かない。
実行時にオプションとして --home を渡してやるらしい。

