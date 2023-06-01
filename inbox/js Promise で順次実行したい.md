---
type: note
---

#js

---
2022-06-22  18:19

# js Promise で順次実行したい

```js
const a = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => { resolve("a") },1000)
  })
}
const b = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => { resolve("b") }, 1000)
  })
}
const myPromise = (num) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(num)
    }, 1000)
  })
}
async function main() {
  const result = await myPromise(10)
  console.log(result)
  const aa = await a()
  console.log(aa)
  const bb = await b()
  console.log(bb)

}
main()
```

1秒ごとに出力される。

これが map とかで何とかならないだろか。

```js
const myPromise = (num) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log(num)
      resolve(num)
    }, 1000)
  })
}

let promise = Promise.resolve()

for(let i = 0; i < 10; i++){
  promise = promise.then(() => {
    return myPromise(i)
  })
}
```

これで、順次1秒ごとに出る。

Promise.resolve()
  .then( () => myPromise(0) )
  .then( () => myPromise(0) )
  ....

こうしたかった。

## 順番に後付けの非同期処理をさせたいとき

```js
const a = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("a")
      resolve()
    },2000)
  })
}
const b = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("b")
      resolve()
    }, 2000)
  })
}
const c = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("c")
      resolve()
    }, 2000)
  })
}

let promise = Promise.resolve()

promise = promise.then( ()=>a())
promise = promise.then( ()=>b())
promise = promise.then( ()=>c())
```

resolve() を返すことで、次に行けるので、注意してつけよう。


