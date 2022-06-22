#vscode #clojure

---
2021-10-22

# Clojure 環境

https://scrapbox.io/blackawa/VSCode%E3%81%A7Clojure%E3%81%AEREPL%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA%E3%82%92%E4%BD%93%E9%A8%93%E3%81%99%E3%82%8B


![[Pasted image 20211022204627.png]]

もうこれだけで、xxx.clj ファイルを開くと、nREPL に自動的に接続される。

![[Pasted image 20211022205837.png]]

コマンドパレットから

Clojure: Eval and Show the Result を

![[Pasted image 20211022205859.png]]

式の後ろで実行すると、

![[Pasted image 20211022210100.png]]

結果が見える。

#### terminal と連携させる

ctrl + shift + @ で Terminal を開いて、repl を起動させる。そのときにポートを合わせる。

```shell
$ lein repl :connect localhost:51434

=> 
```

ここで評価済みのファイルの関数を試せる。
いい感じになってきた。

#### eval などをショートカットに当てる

https://spin.atomicobject.com/2017/06/22/clojure-development-with-visual-studio-code/

#vscode  ショートカットのバインド設定 short cut key

これで、⌘+x  ⌘+e  で評価、結果と出来るようになって、更にいい感じになった。


 ### ちょっと書いてみる
 
 fibonatuchi
 
 ```clojure
(defn fib [x]
  (cond
    (= x 0) 1
    (= x 1) 1
    :else (+ (fib (- x 1)) (fib (- x 2)))))

(defn fibonatuchi []
  (map fib (range)))

(take 20 (fibonatuchi))
(1 1 2 3 5 8 ....)
```

