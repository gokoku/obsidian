#js

---
2021-09-08

# js 代数的パターンマッチのトリック

「関数型プログラミングの基礎」javascript を使って遊ぶ　より、p140

```js
const empty = () => (pattern) => pattern.empty()

const cons = (value, list) =>  (pattern) => pattern.cons(value, list)


const match = (data, pattern) => data(pattern)


const isEmpty = (alist) => match( alist, {
    empty: (_) => (true),
    cons: (head, tail) => (false)
  })


const head = (alist) => match( alist, {
    empty: (_) => (null),
    cons: (head, tail) => (head)
  })

const tail = (alist) => match( alist, {
    empty: (_) => (null),
    cons: (head, tail) => (tail)
  })

const map = (alist, transform) => (
  match(alist, {
    empty: () => empty(),
    cons: (head, tail) => const(transform(head), map(tail, transform))
  })
)

const toArray = (alist) => {
  const toArrayHelper = (alist, accumulator) => (
    match(alist, {
      empty: () => accumulator,
      cons: (head, tail) => toArrayHelper(tail, accumulator.concat(head))
    })
  )
  return toArrayHelper(alist, [])
}
//console.log( isEmpty( empty() ) )
//console.log( isEmpty( cons( 1, empty()) ) )
//console.log( head( cons( 1, empty()) ) )
//console.log( head( tail( cons( 1, cons( 2, empty()))) ) )

var tmp =  ({empty: () => (null), cons: (head, tail)=>(tail)}).cons(1, cons(2, empty()))

// tmp は関数 cons(2, empty())
console.log(  tmp({ empty: () => (null), cons: (head, taild) => (head) })   )          // 2
console.log( ((pattern) => pattern.cons(2, empty())) ({ empty: () => (null), cons: (head, tail) => (head) })   ) // 2
console.log(head(tmp))   // 2

console.log( toArray(map(cons(1, cons(2, cons(3, empty()))), (x)=>(x + 1) )) ) // [ 2, 3, 4 ]
```

return を取ったらなんじゃこりゃ!! 
Haskell 風味になった。

スゲー、js の関数型炸裂トリック。
この人超頭いい。

node v14.17.0



