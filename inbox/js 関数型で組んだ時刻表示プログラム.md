#js

---
2021-09-01

# 関数型で組んだ時刻表示プログラム

## 関数合成関数
```js
const compose =
  (...fns) =>
  (arg) =>
    fns.reduce((composed, f) => f(composed), arg)
```

## データ入出力関数
```js
const oneSecond = () => 1000
const getCurrentTime = () => new Date()
const clear = () => console.clear()
const log = (message) => console.log(message)
```

## Date オブジェクトを独自フォーマットに変換する
### Date オブジェクトから時間オブジェクトに変換する
```js
const serializeClockTime = (date) => ({
  hours: date.getHours(),
  minutes: date.getMinutes(),
  seconds: date.getSeconds(),
})
```

### 時間オブジェクトを(13:00 -> 1:00)に変換する
```js
const civilianHours = (clockTime) => ({
  ...clockTime,
  hours: clockTime.hours > 12 ? clockTime.hours - 12 : clockTime.hours,
})
```
### AM/PM を時間オブジェクトに追加する
```js
const appendAMPM = (clockTime) => ({
  ...clockTime,
  ampm: clockTime.hours >= 12 ? 'PM' : 'AM',
})
```

## 高階関数の実装
### target 関数を使って時刻を表示する
```js
const display = (target) => (time) => target(time)
```

### 文字列フォーマットにしたがって時刻を変換する
```js
const formatClock = (format) => (time) =>
  format
    .replace('hh', time.hours)
    .replace('mm', time.minutes)
    .replace('ss', time.seconds)
    .replace('tt', time.ampm)
```

### 時間オブジェクトのキーを引数にして、10以下なら先頭に0をつける
```js
const prependZero = (key) => (clockTime) => ({
  ...clockTime,
  [key]: clockTime[key] < 10 ? '0' + clockTime[key] : '' + clockTime[key],
})
```
これ謎、[key]でオブジェクトの key がとれるのか

## これらの合成関数
### 関数合成で時間オブジェクトを午前午後形式に変換
```js
const convertToCivilianTime = (clockTime) =>
  compose(appendAMPM, civilianHours)(clockTime)
```

### 先頭に適宜 0 をつける
```js
const doubleDigits = (civilianTime) =>
  compose(
    prependZero('hours'),
    prependZero('minutes'),
    prependZero('seconds'),
  )(civilianTime)
```

### 全部合成して時間を表示
```js
const startTicking = () =>
  setInterval(
    compose(
      clear,
      getCurrentTime,
      serializeClockTime,
      convertToCivilianTime,
      doubleDigits,
      formatClock('hh:mm:ss tt'),
      display(log),
    ),
    oneSecond(),
  )

startTicking()
```

![[Pasted image 20210901153127.png]]