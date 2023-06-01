---
type: note
---

#js

---
2022-07-01  15:20

# js Watch Dog Timer を実装したい

一定時間更新されなかったら、強制リセットするようにする。

遠野AI に実装したい。


```js

class WatchDogTimer {
  constructor(time, callback) {
    this.time = time
    this.currentTime = time
    this.callback = callback
    this.timer = null
  }

  start() {
    this.timer = setInterval(() => {
      this.currentTime--;
      if (this.currentTime <= 0) {
        this.stop()
      }
    }, 1000)
  }

  stop() {
    if(this.callback) this.callback()
    clearInterval(this.timer)
    this.currentTime = this.time
  }
}

const reset = () => {　console.log("reset") }

const watch = new WatchDogTimer(10, reset)
watch.start()
```

これで、途中で止めてイベントを破棄した後でも、また再開できる。

```js

const watch = new WatchDogTimer(10, reset)

watch.start()
console.log("start")

setTimeout(() => {
	watch.stop()
	watch.start()
	console.log("start")
},5000)
```

10秒でリセットがかかるとして、5秒でstopする。
その後で start すると 10秒でリセットがかかる。

この class を実装する。

