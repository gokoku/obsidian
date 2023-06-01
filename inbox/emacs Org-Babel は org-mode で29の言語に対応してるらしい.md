#emacs

---
2021-09-07

# Org-Babel とは

https://ameblo.jp/concello/entry-10776761364.html

Org mode でコードがうめ込める。

- コードブロックの対話的・ファイル上での結果出力  
- パラメーター化されたコードブロックを、他のブロックから呼び出せます。  
- コードの実行結果を編集できる形で出力できます。

# 手始めにいじってみた

M-x package-list-packages で org  gnu をインストールしてみた。

最新の org では Babel が標準とのことだったので。

使う言語をあらかじめ設定するらしい。

init.el
```lisp
;;;-----------------------------------------------------------------------
;; Org mode
(org-babel-do-load-languages
 'org-babel-load-languages
 '((R . t)
   (emacs-lisp . t)
   (haskell . t)
   (python . t)
   (ruby . t)
   (js . t))
 )
(setq org-babel-js-cmd "/Users/george/.asdf/shims/node")   <--- js はこれで動いた。
```

M-x org-mode

```lisp

#+begin_src ruby
require 'date'
"This file was last evaluated on #{Date.today}"
#+end_src

↑ここのブロックにカーソルを入れて、C-c C-c にすると


#+RESULTS:
: This file was last evaluated on 2021-09-07


結果が出た!! 
```

これは Jupyter notebook じゃ!!

# ちゃんと Org モードを使えるようになることにする



コードブロックを出したい。

C-c C-, で s で出せる。

#+begin_src 
#+end_src


# leaf で動かすようにする

```lisp
;; Org mode
(leaf org-babel
  :config
  (org-babel-do-load-languages
   'org-babel-load-languages
   '((R . t)
     (emacs-lisp . t)
     (haskell . t)
     (python . t)
     (ruby . t)
     (shell . t)
     (js . t)
     (java . t)
     (awk . t)))
  :custom
  ((org-babel-js-cmd . "/Users/george/.asdf/shims/node")
   (org-babel-js-function-wrapper .
      "console.log(require('util').inspect(function(){\n%s\n}(), { depth: 100 }))")))
```

orb-babel-js-cmd は必要だった。

何故か、家のmac では org-babel-js-function-wrapper が必用だった。

### java

```java
#+headers: :classname HelloWorld
#+begin_src java :results output :exports both

public class HelloWorld {
  public static void main(String[] args) {
    System.out.print("hello, world");
  }
}

#+end_src
```

これでいける。
java コンパイルが走って、生成ファイルが出来てる。

### haskell

package-list-packages で haskell-mode と inferior-haskell-mode をインストールしたら動いた。

```haskell
#+begin_src haskell

let x = "Hello world."
putStrLn x

#+end_src

#+RESULTS:
: Hello world.
```

スゲー。

```haskell

#+BEGIN_SRC haskell

[x*2 | x <- [1..10], x*2 >= 12]

#+END_SRC

#+RESULTS:
| 12 | 14 | 16 | 18 | 20 |
```

この場合はこう。
```haskell

#+BEGIN_SRC haskell

:{
caseOfFirstLetter :: String -> String

caseOfFirstLetter str =
  case str of
    ""  -> ""
    (x:xs) | 'a' <= x && x <= 'z' -> "lower"
           | 'A' <= x && x <= 'Z' -> "upper"
           | otherwise            -> "other"
:}
caseOfFirstLetter "test"

#+END_SRC

#+RESULTS:
: lower
```

ghci にとおしているので、:{  : } がないとエラーになる。


