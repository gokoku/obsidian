---
type: note
---

#clojure 

---
2023-05-02  14:05

# clojure loop と recur による再帰の使い方

loop が recur のエントリーポイントになる
```clojure
(loop [result [] x 5]
  (if (zero? x)
    result
    (recur (conf result x) (dec x)))) => [5 4 3 2 1]
```

loop を使わないと、関数頭がエントリーポイントになる

```clojure
(defn countdown [result x]
  (if (zero? x)
    result
    (recure (conj result x) (dec x))))

(countdown [] 5) => [5 4 3 2 1]
```


しかし、あまり使われないらしい。
何故なら、使わなくてもいいらしいから。

```clojure
(into [] (take 5 (iterate dec 5))) => [5 4 3 2 1]

(into [] (drop-last (reverse (range 6))))
(into [] (reverse (rest (range 6))))
```