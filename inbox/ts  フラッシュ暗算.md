#ts

---
2021-12-14

# フラッシュ暗算

```shell
$ mkdir flash
$ cd flash
$ npm init -y
$ npm i -D typescript@4.3.5

$ mkdir src
$ touch src/index.ts
```

Promise ベースにしたかった。

待ち時間が段々長くなるようにしてインターバルを持たせた。

入力を await で待つ。

```
promise[0] 
promise[1] -
promise[2] --
promise[3] ---
promise[4] ----
promise[5] -----
answer     -----
```


この標準入出力はいいな。

index.ts

```js
const printLine = (text: string, breakLine: boolean = true, crTimes: number= 1) => {
  process.stdout.write(text)
  if(breakLine) {
    [...Array(crTimes)].forEach(() => process.stdout.write('\n'))
  } else {
    process.stdout.write(' ')
  }
}
const readLine = async () => {
  const input: string = await new Promise((resolve) =>
    process.stdin.once('data', (data) => resolve(data.toString())))
  return input.trim()
}

const promptInput = async (text: string) => {
  printLine(`\n${text}\n> `, false)
  return readLine()
}

class Anzan {

  private sec = 700
  private digit = 1
  private times = 10
  private result: number[] = []

  setting() {
    [...Array(this.times)].forEach(() => {
      let tmp = (Math.floor(Math.random() * (10**this.digit)))
      this.result.push(tmp === 0 ? 1 : tmp)
    })
    //console.log(this.result)
  }
  play() {
    printLine("スタート", true, 2)
    this.result.forEach((val, index) => {
      // timeout の間隔が増えていく Promise を生成
      new Promise(() => {
        setTimeout(() => {
          printLine(val.toString(), true, 2)
        }, this.sec * index)
      })
    })
  }
  answer() {
    setTimeout(async () => {
      const answer = await promptInput('答えは?')
      const sum = this.result.reduce((a, b) => a + b, 0)
      if( sum === Number(answer)) {
        printLine("\n(((🟠)))", true, 4)
      } else {
        printLine(`\n--- ${sum} ---`, true, 2)
      }
      process.exit(0)
    }, this.sec * this.result.length)

  }
}
(async () => {
  const anzan = new Anzan()
  anzan.setting()
  await anzan.play()
  await anzan.answer()

})()
```

```shell
$ npm run build
$ npm run start
```

