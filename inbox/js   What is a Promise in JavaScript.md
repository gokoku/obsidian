#js

---
2021-07-19

# What is a Promise in JavaScript?

https://dmitripavlutin.com/what-is-javascript-promise/

## 同期処理から
```js
function getList() {
  return ['Joker', 'Batman']
}
function findPerson(who) {
  const list = getList()
  const found = list.some((person) => person === who)
  console.log(found)
}

findPerson('Joker')
```


## 非同期どー処理する?

```js
function getList(callback) {
  setTimeout(() => {
    ['Joker', 'Batman']
  }, 1000)
}
```

処理時間がDelay するとこのままではうまく行かない。

### コールバックで対処する

```js
function getList(callback) {
  setTimeout(() => {
    callback(['Joker', 'Batman'])
  }, 1000)
}
function findPerson(who) {
  getList((list) => {
    const found = list.some((person) => person === who)
    console.log(found)
  })
}

findPerson('Joker')
```

めちゃくちゃ可読性が悪くなって、早晩コールバック地獄になる。


### Encapsulating asynchronicity

非同期処理をカプセル化するのが、promise object とのこと。

Promises (非同期処理結果をラップしたもの)は関数からの同期的に返される。
変数にアサインしたり、引数として使ったりしする。

Promises のアイディア : 非同期のカプセル化と非同期処理のハンドリングの許可できて同期処理に見える形


![[Pasted image 20210719130156.png]]

```js
const promise = new Promise((resolve, reject) => {
  if (asyncOperationSuccess) {
    resolve(value)
  } else {
    reject(error)
  }
})
```

というわけで、 getList() をこう書く。

```js
function getList() {
  return new Promise(resolve => {
    setTimeout(() => resolve(['Joker', 'Batman']), 1000)
  })
}
```

#### Extracting the promise fulfill value

Promise の fulfill value を抽出するには、

```js
promise
  .then(value => { .... })
```

![[Pasted image 20210719131446.png]]

というわけで、promise を使ったコード。

```js
function getList() {
  return new Promise((resolve) => {
    setTimeout(() => resolve(['Joker', 'Batman']), 1000)
  })
}

function findPerson(who) {
  const listPromise = getList()
  listPromise.then((list) => {
    const found = list.some((person) => person === who)
    console.log(found)
  })
}

findPerson('Joker')
```

#### Extracting the promise rejection error

```js
promise
  .catch(error => {
	//check error...
  })
```

![[Pasted image 20210719131910.png]]

```js
function getList() {
  return new Promise((resolve, reject) => {
    setTimeout(() => reject(new Error('Nobody here!')), 1000);
  });
}

const promise = getList();

promise
  .catch(error => {
    console.log(error); // logs Error('Nobody here!')
  });
```

#### Extracting value and error

どちらも抽出には 2 way ある。

##### promise.then(successCallback, errorCallback)

```js
promise
  .them(value => {
	// use value...
  }, error => {
	// check error...
  })
```

##### promise.then(successCallback).catch(errorCallback)

```js
promise
  .then(value => {
	// use value...
  })
  .catch(error => {
	// check error...
  })
```

成功パターン。

```js
function getList() {
  return new Promise((resolve) => {
    setTimeout(() => resolve(['Joker', 'Batman']), 1000)
  })
}

const promise = getList()

promise
  .then((value) => {
    console.log(value)
  })
  .catch((error) => {
    console.log(error)
  })

```

エラーバターン。

```js
function getList() {
  return new Promise((resolve, reject) => {
    setTimeout(() => reject(new Error('Nobody here!')), 1000)
  })
}

const promise = getList()

promise
  .then((value) => {
    console.log(value)
  })
  .catch((error) => {
    console.log(error)
  })
```

## Chain of promises

```js
function delayDouble(number) {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve(2 * number), 1000)
  })
}

delayDouble(5)
  .then((value1) => {
    console.log(value1)
    return delayDouble(value1)
  })
  .then((value2) => {
    console.log(value2)
    return delayDouble(value2)
  })
  .then((value3) => {
    console.log(value3)
  })
  .catch((error) => {
    console.log(error)
  })
```

![[Pasted image 20210719133903.png]]

## async/await

promise の中でもトップな、本当に使い勝手のいいシンタックスシュガー。

できるなら、これが最高にオススメとのこと。

### await-ing promise value

promise が成功したなら、await promise 式での 成功値の評価はこうなる。

```js
async function myFunction() {
  const value = await promise
}
```

![[Pasted image 20210719135051.png]]

```js

function getList() {
  return new Promise((resolve) => {
    setTimeout(() => resolve(['Joker', 'Batman']), 1000)
  })
}

async function findPerson(who) {
  const list = await getList()

  const found = list.some((person) => person === who)
  console.log(found)
}

findPerson('Joker')

```

### catch-ing promise error

```js
async function myFunction() {
  try {
    const value = await promise
  } catch (error) {
    error
  }
}
```

![[Pasted image 20210719135523.png]]

```js
function getList() {
  return new Promise((resolve, reject) => {
    setTimeout(() => reject(new Error('Nobody here!')), 1000)
  })
}

async function findPerson(who) {
  try {
    const list = await getList()
    const found = list.some((person) => person === whoe)
    console.log(found)
  } catch (error) {
    console.log(error)
  }
}

findPerson('Joker')
```

### await-ing chain

```js
function delayDouble(number) {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve(2 * number), 1000)
  })
}

async function run() {
  const value1 = await delayDouble(5)
  console.log(value1)

  const value2 = await delayDouble(value1)
  console.log(value2)

  const value3 = await delayDouble(value2)
  console.log(value3)
}

run()

```

非同期が見通しの良いコードになる!!
