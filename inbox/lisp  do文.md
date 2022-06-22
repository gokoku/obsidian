#lisp



```lisp
(dolist (x '(1 2 3))
  (print x))
1
2
3
NIL
```

```lisp
(dotimes (i 4)
  (print i))
0
1
2
3
4
NIL
```

```lisp
(let ((a (loop for i from 1 to 10
               collecting i)))
     (print a))
(1 2 3 4 5 6 7 8 9 10)
```

## フィボナッチ数列

```lisp
(do ((n 0 (1+ n))
     (cur 0 next)
     (next 1 (+ cur next)))
  ((= 10 n) (print cur)))

55
```

```lisp
(loop for i below 10
      and a = 0 then b
      and b = 1 then (+ b a)
      finally (return (print a)))
55
```
