[---
type: note
---

#pop_os #utuntu

---
2023-02-07  21:14

# pop_os _apt からアクセスできないため・・・

deb ファイルを直に apt install でやると、_apt からアクセスできないため、ダウンロードは root でサンドボックスを通さずに行われます。

となる。

```shell
$ sudo apt install gdebi

$ sudo gdebi debファイル
```