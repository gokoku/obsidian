---
type: note
---

#clojure 

---
2023-05-02  14:00

# clojure  do の副作用の使い方

if の分岐先は一つの式しか書けないので、do で複数のアクションを起こす。

```clojure
(defn is-small? [number]
 (if (< number 100)
   "yes"
   (do
     (println "Saw a big number" number)
     "no")))
```

```clojure
(is-small? 5000)
Saw a big number 5000
"no"
```

