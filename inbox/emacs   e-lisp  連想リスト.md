#emacs #lisp 

---
2021-10-27


# 連想リスト alist

alist

```lisp
((rose . "red") (violet . "blue") (lily . "white")))
```

assq を使ってドット対を検索する

```lisp
(assq 'lily '((rose . "red") (violet . "blue") (lily . "white")))
(lily . "white")
```

assq の実装を考える。

```lisp
(defun Assq (key alist)
  (cond
    ((null alist) nil)
    ((eq key (car (car alist))) (car alist))
    (t (Assq key (cdr alist)))))
```

リストが等しいか判別する

```lisp
(defun squiv (x y)
  (cond
   ((and (null x) (null y)) t)
   ((or (null x) (null y)) nil)
   (t (and (car x) (car y)
           (squiv (cdr x) (cdr y))))))


(squiv '(1 t dog) '(1 t dog))
t
```

最初の and 条件で同じ長さで合致したとき、
or でどちらかが長さが無くなったとき、

「x の先頭の要素と y の先頭の要素が等しく」

「x の残りと y の残りが等しい」こと。

