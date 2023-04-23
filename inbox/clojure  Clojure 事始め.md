#clojure

---
2021-09-03

# Clojure 事始め

asdf で clojure と lein をいれた。

#chrome_book   asdf-vm はすごそう]]

```shell
$ asdf plugin add clojure https://github.com/asdf-community/asdf-clojure.git


```

globalをセットしてないと動かない。

```shell
$ asdf global lein 2.9.6

$ lein new sample
$ cd sample
```

project.clj で main を設定する。

```clojure
  :main sample.core)
```

core.clj で foo を -main にする。
```clojure
(ns sample.core)

(defn -main
  "I don't do a whole lot."
  [x]
  (println x "Hello, World!"))
```

```shell
$ lein deps   関連ファイルをダウンロード

$ lein run george
george Hello, World!
```

## 書いてみる

```shell
$ lein repl

>(map (fn [x] (+ x 1)) '(1 2 3)')
(2 3 4)
```
