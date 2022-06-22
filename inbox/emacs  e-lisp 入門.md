#emacs #lisp 

---
2021-10-27


# e-lisp 入門

```lisp
(defun sum (lst)
  (cond
   ((null lst) 0)
   (t (+ (car lst) (sum (cdr lst))))))

(sum '(1 2 3))
```

リストの要素をリプレイス
```lisp
(defun repl (lst old new)
  (cond
   ((null lst) ())
   ((eq (car lst) old)
    (cons new (repl (cdr lst) old new)))
   (t (cons (car lst) (repl (cdr lst) old new)))))

(repl '(dog pig dog) 'dog 'rat)
(rat pig rat)
```

リストの要素を削除する

```lisp
(defun del (x lst)
  (cond
   ((null lst) ())
   ((eq x (car lst)) (del x (cdr lst)))
   (t (cons (car lst) (del x (cdr lst))))))


(del 'dog '(dog pig dog rat))
(pig rat)
```

メンバーの中に要素があるか

```lisp
(defun memq (x lst)
  (cond
   ((null lst) ())
   ((eq x (car lst)) t)
   (t (memq x (cdr lst)))))


(memq 3 '(1 2 3))
t
(memq 'dog '(pig rat))
```

memq を使って、リストの要素がリストの中に全部あるか

```lisp
(defun subset (set1 set2)
  (cond
   ((null set1) t)
   (t (and (memq (car set1) set2) (subset (cdr set1) set2)))))

(subset '(1 2) '(2 4 3 1))
t
```
