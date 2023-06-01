---
type: note
---

#ts 

---
2022-06-13  16:00

# typescript  Repl コマンドツール ts-node

```shell
$ npm -g i ts-node

$ ts-node
> enum FileAccess {
> ...None,
> ...Read = 1 << 1,
> ...Write = 1 << 2
> }
> let x: FileAccess = FileAccess.Read
> x
2
> x |= FileAccess.Write
6
```

素晴らしい。
