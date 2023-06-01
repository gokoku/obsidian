#emacs #lisp

---
2021-10-27

#  e-lisp 二項演算

こんな感じに。

```lisp
(calc '((1 + 2) - (3 * 4)))
```

```lisp
(defun calc (exp)
  (cond
   ((atom exp) exp)
   ((eq (car (cdr exp)) '+)
    (+ (calc (car exp))
       (calc (car (cdr (cdr exp))))))
   ((eq (car (cdr exp)) '-)
    (- (calc (car exp))
       (calc (car (cdr (cdr exp))))))
   ((eq (car (cdr exp)) '*)
    (* (calc (car exp))
       (calc (car (cdr (cdr exp))))))))



(calc '((1 + 2) - (3 * 4)))
-9
```

## 補助関数を使ってリファクタリング。

```lisp
(defun op (exp) (car (cdr exp)))
(defun arg1 (exp) (car exp))
(defun arg2 (exp) (car (cdr (cdr exp))))
       


(defun calc (exp)
  (cond
   ((atom exp) exp)
   ((eq (op exp) '+)
    (+ (calc (arg1 exp)) (calc (arg2 exp))))
   ((eq (op exp) '-)
    (- (calc (arg1 exp)) (calc (arg2 exp))))
   ((eq (op exp) '*)
    (* (calc (arg1 exp)) (calc (arg2 exp))))))

(calc '(1 + 2) + (1 + 2)')
6
(calc '((1 + 2) - (3 * 4)))
-9
```

## funcall と連想リストを使って演算を抽象化

```lisp
(defun op (exp) (car (cdr exp)))
(defun arg1 (exp) (car exp))
(defun arg2 (exp) (car (cdr (cdr exp))))

(setq op-func1 '((+ . +) (- . -) (* . *)))
(defun op-func (sym op-db)
  (cdr (assq sym op-db)))


(defun calc (exp op-db)
  (cond
   ((atom exp) exp)
   (t
    (funcall (op-func (op exp) op-db)
             (calc (arg1 exp) op-db)
             (calc (arg2 exp) op-db)))))



(calc '((1 * 3) - ( 2 * 3)) op-func1)
-3
```

連想リストを変えるとこうも出来るようになった。

```lisp
(setq op-func2 '((add . +) (sub . -) (mul . *)))
(calc '((1 add (2 mul 3)) sub 3) op-func2)
4
```

## 更なる抽象化

こんな感じで形式まで変えられる。

```lisp

(calc '(1 + (2 * 3)) op-func1 order-func1)

(calc '(+ 1 (* 2 3)) op-func1 order-func2)

(calc '(add 1 (mul 2 3)) op-func2 order-func2)
```

```lisp
(defun 1st (exp) (car exp))
(defun 2nd (exp) (car (cdr exp)))
(defun 3rd (exp) (car (cdr (cdr exp))))

(setq op-func1 '((+ . +) (- . -) (* . *)))
(setq op-func2 '((add . +) (sub . -) (mul . *)))
(setq order-func1 '((OP . 2nd) (ARG1 . 1st) (ARG2 . 3rd)))
(setq order-func2 '((OP . 1st) (ARG1 . 2nd) (ARG2 . 3rd)))

(defun op-func (sym op-db)
  (cdr (assq sym op-db)))

(defun order-func (sym odr-db)
  (cdr (assq sym odr-db)))

(defun op (exp order-db)
  (funcall (order-func 'OP order-db) exp))

(defun arg1 (exp order-db)
  (funcall (order-func 'ARG1 order-db) exp))

(defun arg2 (exp order-db)
  (funcall (order-func 'ARG2 order-db) exp))
  

(defun calc (exp op-db odr-db)
  (cond
   ((atom exp) exp)
   (t (funcall
     (op-func (op exp odr-db) op-db)
     (calc (arg1 exp odr-db) op-db odr-db)
     (calc (arg2 exp odr-db) op-db odr-db)))))

```





