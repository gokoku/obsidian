---
type: note
---

#lisp #roswell

---
2022-06-08  16:20

# lisp プロジェクトを作る


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/t-sin/items/054c2ff315ec3b9d3bdc" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9JUUzJTgxJTg0JUUzJTgxJUJFJUUzJTgxJThCJUUzJTgyJTg5JUU1JUE3JThCJUUzJTgyJTgxJUUzJTgyJThCQ29tbW9uJTIwTGlzcCZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9ZmU0ODhiMzY3NTQ5MjNlYTQ5ZTNjYTVmNDMzYWZmZGQ&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwdC1zaW4mdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPWUxMTgyZDQ2NTVmNzQwOTk4MGE2Y2Y3NGJhZGQ1MTEx&blend-x=142&blend-y=491&blend-mode=normal&s=1d13db8197e7473a16f174896947ec6a')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">いまから始めるCommon Lisp - Qiita</h1>
		<p class="rich-link-card-description">
		この記事はLisp Advent Calendar 2017の二日目の記事です。 はじめに  この記事は、Common Lispという初めての人には初めましてな言語の入門記事です。  この世には、Common Lispというとって...
		</p>
		<p class="rich-link-href">
		https://qiita.com/t-sin/items/054c2ff315ec3b9d3bdc
		</p>
	</div>
</a></div>

roswell のプロジェクトは `~/.roswell/local-projects` の中に作るそうです。

```shell
$ cd ~/.roswell/local-projects
$ mkdir -p cl-cat/roswell
$ cd cl-cat/roswell
$ ros init cat
```

```
cl-cat
├── cl-cat.asd
├── main.lisp
└── roswell
    └── cat.ros
```

cl-cat.asd
```lisp
;;; cl-cat.asd
(in-package :cl-user)    ; どのパッケージにいるかわからないので CL-USER パッケージにする
(defpackage :cl-cat-asd  ; ASDFのシステム定義用パッケージを作る
  (:use :cl :asdf))       ; 標準関数とASDFの関数をパッケージ修飾なしに呼べるようにする
(in-package :cl-cast-asd)  ; 作ったパッケージにする

(defsystem :cl-cat
    :class :package-inferred-system ; システム定義のスタイル
    :description "cat command implemented with Common Lisp"
    :version "0.1"
    :author "george"
    :license "Public Domain"
    :depends-on ("cl-cat/main"))   ; ソースファイルの指定
```

`roswell/cat.ros`
```lisp
#!/bin/sh
#|-*- mode:lisp -*-|#
#|
exec ros -Q -- $0 "$@"
|#
(progn ;;init forms
  (ros:ensure-asdf)
  #+quicklisp(ql:quickload '(:cl-cat) :silent t)
  )

(defpackage :ros.script.cat.3863662032
  (:use :cl))
(in-package :ros.script.cat.3863662032)

(defun main (&rest argv)
  (declare (ignorable argv))
  (if (>= (length argv) 1)
      (with-open-file (in (first argv))
        (cl-cat:cat in *standard-output*))
      (cl-cat:cat *standard-input* *standard-output*)))
;;; vim: set ft=lisp lisp:
```

```shell
$ cd roswell
$ ros build cat.ros

$ ./cat cat.ros
```

うん、動いた。
clojure みたいだ。