#lisp 


```lisp
(defun plot (fn min max step)
  (loop for i from min to max by step do
        (loop repeat (funcall fn i) do (format t "*"))
        (format t "~%")))
```

```shell
    $ (plot #'exp 0 4 1/2)
    *
    **
    ***
    *****
    *******
    **************
    ***********************
    *******************************************
```


```shell
$ (plot #'(lambda (x) (* (sin x) 50)) 0 3 0.1)

```
