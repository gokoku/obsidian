#command #web

---
2021-07-02

# BOM

UTF-8 の BOM は、 0xEF 0xBB 0xBF

頭にBOMついたCSVファイルを読み込むときがあるので注意。

```shell
$ hexdump -C some.csv

EF BB BF ....
```
