---
type: note
---

#js

---
2022-06-23  18:26

# js RxJS を使いたい

https://www.codegrid.net/articles/2017-rxjs-1/

これは、Draw tool でイベントをまとめるのがいいかもしれない。
遠野AI ではまだ要らないようだ。

```shell
$ npm i jxjs
```

![[RxJS 2022-06-24 13.38.51.excalidraw | 400]]

Obserbable から subscribe するパターン

```js
import { fromEvent, Subject, Observable, from, of, interval } from "rxjs"


const observable = interval(1000)
const subscription = observable.subscribe(val => console.log(val))

setTimeout(() => subscription.unsubscribe(), 5000)
```

Operator で加工するパターン

```js
import { fromEvent, Subject, Observable, from, of, interval } from "rxjs"
import { map, scan } from "rxjs/operators"


fromEvent(document, 'click')
  .pipe(
    map(e => 1),
    scan((acc, curr) => acc + curr, 0)
  )
  .subscribe(x => console.log(x))
```


Subject で複数 subscribe するパターン

```js
const subject = new Subject()

subject.subscribe(x => console.log('A', x))
subject.subscribe(x => console.log('B', x))

const observable = from([1,2,3])
observable.subscribe(subject)
```

Subject が Observable になるパターン

```js

import { fromEvent, Subject, Observable, from, of, interval } from "rxjs"
import { map, scan } from "rxjs/operators"

const subject = new Subject()

subject.subscribe(x => console.log('A', x))
subject.subscribe(x => console.log('B', x))

subject.next(1)
subject.next(2)
subject.next(3)
```


## Event をまとめたいのだが

event を集めて、ディシジョンテーブルで、処理を管理したいのだが。

![[RxJS 2022-06-24 14.33.51.excalidraw | 600]]


```js

import { fromEvent, Subject, Observable, from, of, interval } from "rxjs"


const observable = interval(100)
observable.subscribe(x => decisionTable.judge())

fromEvent(document, 'click').subscribe(x => decisionTable.click())
fromEvent(document, 'keydown').subscribe(x => decisionTable.keydown(x.key))

class DecisionTable {
  constructor(){
    this.clickFlag = false
    this.keydownFlag = false
  }
  judge() {
    if(this.clickFlag && this.keydownFlag){
      console.log('click and keydown', this.keydownFlag)
    }
    this.clear()
  }
  clear() {
    this.clickFlag = false
    this.keydownFlag = false
  }
  click() {
    this.clickFlag = true
  }
  keydown(key) {
    this.keydownFlag = key
  }
}

const decisionTable = new DecisionTable()
```

