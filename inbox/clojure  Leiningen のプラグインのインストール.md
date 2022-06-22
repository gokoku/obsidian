#clojure

---
2021-10-25

# Leiningen のプラグイン

~/.lein/profiles.clj に書き込むと、一々プロジェクト毎にインストールしなくていいと。

pprint の例。

https://github.com/technomancy/leiningen/tree/master/lein-pprint

```json
{:user {:plugins #lein-pprint "1.3.2"]]}}
```

```shell
$ lein help

pprint              Usage: pprint [--no-pretty] [--] [selector...]

```


### Clojars でライブラリを探す

https://clojars.org/

![[Pasted image 20211025133005.png]]

最新バージョンが見られる。



