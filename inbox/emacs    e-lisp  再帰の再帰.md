#emacs #lisp

---
2021-10-27

# 再帰の再帰


```lisp
(sum* '(1 2 (3 4) (5)))
```

```lisp
(defun sum* (lst)
  (cond
   ((null lst) 0)
   ((consp (car lst))
    (+ (sum* (car lst)) (sum* (cdr lst))))
   (t (+ (car lst) (sum* (cdr lst))))))


(sum* '(1 2 (3 4 (5)) 6 (7 8 9) 10))
55
```

consp でリストだったら
```lisp
(+ (sum* (car lst) (sum* (cdr lst))))
```

これで再帰の中の再帰まで届くから驚きだ。

---
 
リプレイスの再帰の再帰

```lisp

(defun repl (lst old new)
  (cond
   ((null lst) nil)
   ((consp (car lst))
    (cons (repl (car lst) old new)
          (repl (cdr lst) old new)))
   ((eq old (car lst)) (cons new (repl (cdr lst) old new)))
   (t (cons (car lst) (repl (cdr lst) old new)))))

(repl '(1 2 3 (5 4) (6 (3 (2 4))) 7) 4 10)
(1 2 3 (5 10) (6 (3 (2 10))) 7)
```

凄い!!

---

削除の再帰の再帰

```lisp
(defun del* (x lst)
  (cond
   ((null lst) nil)
   ((consp (car lst))
    (cons (del* x (car lst))
          (del* x (cdr lst))))
   ((eq x (car lst))
    (del* x (cdr lst)))
   (t (cons (car lst) (del* x (cdr lst))))))




(del 3 '(1 2 (4 6 (3)) 3))
(1 2 (4 6 nil))

```

(3) から 3削除されて () が nil となってたのか。

---

集合から要素があるかの再帰

```lisp
(defun memq* (x set)
  (cond
   ((null set) nil)
   ((consp (car set))
    (or (memq* x (car set))
        (memq* x (cdr set))))
   ((eq x (car set)) t)
   (t (memq* x (cdr set)))))


(memq* 5 '(1 4 (2 3 (9 5))))
t

```

リストが等しいか

```lisp

(defun equiv (x y)
  (cond
   ((and (null x) (null y)) t)
   ((or (null x) (null y)) nil)
   ((and (consp (car x)) (consp (car y)))
    (and (equiv (car x) (car y))
         (equiv (cdr x) (cdr y))))
;;   ((or (consp (car x)) (consp (car y))) nil)
   (t (and (eq (car x) (car y)) (equiv (cdr x) (cdr y))))))




(equiv '(1 2 (4 (5)) 3) '(1 2 (4 (5)) 3))
t

(equiv '(dog (pig (rat))) '(dog (pig (rat))))
t

```
;; の or  が無くてもいい気がするのだが、本には書いていた。

