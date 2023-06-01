#command 

---
2021-09-13

# POSIX 準拠のシェルスクリプトでは find | xargs よりも find -exec {} + を使うべき！

https://qiita.com/ko1nksm/items/9ff1f212255e8520070c

だそうです。

find | xargs はなんか覚えにくい。

```shell
$ find . -type f -exec printf "%s\n" {} +
```

