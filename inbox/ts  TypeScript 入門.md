#ts

---
2021-12-12

# TS 入門

```shell
$ npm -g i typescript
```

tsc コマンド、tsserver  コマンドが使えるようになった。

sum.ts

```ts
function sum(a, b) {
    return a + b;
}
console.log(sum(1, 2));
```

```shell
$ tsc sum.ts

$ node sum.js
3

```

sum.js が出来る。

```shell
$ tsc --init
```

.tsconfig.json が作られる。

default では strictNullCheck が true になってるような。(version 4.5.3) 

明示的に指定しておくらしいが。

面倒くさいから deno で直に実行したい。

[[deno  Dino 入れてみる]]

```shell
$ deno run sum.ts
```

## Node.js で動くアプリケーション

```shell
$ mkdir node-app
$ cd node-app
$ npm init -y
$ npm i -D typescript@4.3.5

$ mkdir src
$ touch src/index.ts
```

src/index.ts

```ts
const sayHello = (name: string) => {
  return `Hello ${name}`
}

console.log(sayHello('World'))
```

package.json

```json

  "scripts": {
    "build": "tsc",
    "dev": "tsc -w",
    "start": "node dist/index.js"
  },
```

```shell
$ touch tsconfig.json
```

tsconfig.json

```json
{
  "compilerOptions": {
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true
  }
}
```

```shell
$ npm run build
$ npm run start
```

#### process.stdout.write にする

index.ts

```ts
const sayHello = (name: string) => {
  return `Hello ${name}`
}

process.stdout.write(sayHello('World'))
```

process がないのでインストール。

```shell
$ npm install -D @types/node@16.4.13

$ npm run build
$ npm run start

Hello World
```

### 対話型の関数

ウォッチしながらコンパイル。

```shell
$ npm run dev
```

src/index.ts

```ts
const printLine = (text: string, breakLine: boolean = true) => {
  process.stdout.write(text + (breakLine ? '\n' : ''))
}

const promptInput = async (text: string) => {
  printLine(`\n${text}`, false)
  const input: string = await new Promise((resolve) => process.stdin.once('data', (data) =>
    resolve(data.toString())))
  return input.trim()
}

(async () => {
  const name = await promptInput('What is your name? ')
  console.log(name)
  const age = await promptInput('How old are you? ')
  console.log(age)
  process.exit(0)
})()
```

```shell
$ npm run build
$ npm run start

What is your name? Tada
Tada

How old are you? 29
29
```

## Hit and Blow Game

メソッドの中で、自分を呼び出すところがどうも変だ。

コンパイル動作が変だったな。前のキャッシュでも残ってるような感じ。

動作も変。不正を出した分だけスタックに積まれて正解してもその回数分不正解になった。

await this.play() がジャンプではなくて、再入って感じの動作??に近かった。

なので、正解したら return this.end() で強制終了させたら上手くいった。

やっぱり、不正解で再入した分だけ return してきて、無くなって終了する動作だ。

index.ts

```ts
const modes = ['normal', 'hard'] as const
type Mode = typeof modes[number]

const printLine = (text: string, breakLine: boolean = true) => {
  process.stdout.write(text + (breakLine ? '\n' : ''))
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

const promptSelect = async <T extends string>(text: string, values: readonly T[]): Promise<T> => {
  printLine(`\n${text}`)
  values.forEach((value) => {
    printLine(`- ${value}`)
  })
  printLine(`> `, false)

  const input = await readLine() as T
  if (values.includes(input)) {
    return input
  } else {
    return promptSelect<T>(text, values)
  }
}

class HitAndBlow {
  private readonly answerSource = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
  private answer: string[] = []
  private tryCount = 0
  private mode: Mode = 'normal'

  async setting() {
    this.mode = await promptSelect<Mode>('モードを選択してください。', modes)
    const answerLength = this.getAnswerLength()
    while(this.answer.length < answerLength) {
      const randNum = Math.floor(Math.random() * this.answerSource.length)
      const selectedItem = this.answerSource[randNum]
      if(!this.answer.includes(selectedItem)) {
        this.answer.push(selectedItem)
      }
    }
  }

  async play() {
    const answerLength = this.getAnswerLength()
    const inputArr = (await promptInput(`「,」区切りで${answerLength}つの数字を入力してください `)).split(',')

    if(inputArr.every((val) => val === 'q')) {
      console.log(this.answer)
      await this.play()
    } else if(!this.validate(inputArr)) {
      printLine('入力が不正です。')
      await this.play()
    }
    const result = this.check(inputArr)
    if (result.hit !== this.answer.length) {
      printLine(`---\nHit: ${result.hit}\nBlow: ${result.blow}\n---`)
      this.tryCount += 1
      await this.play()
    } else {
      this.tryCount += 1
      return this.end()
    }
  }

  end() {
    printLine(`正解です。You tried ${this.tryCount} times.`)
    process.exit(0)
  }
  private check(input: string[]) {
    let hitCount = 0
    let blowCount = 0
    input.forEach((val, index) => {
      if (val === this.answer[index]) {
        hitCount++
      } else if (this.answer.includes(val)) {
        blowCount++
      }
    })
    return {
      hit: hitCount,
      blow: blowCount
    }
  }
  private validate(inputArr: string[]) {
    const isLengthValid = inputArr.length === this.answer.length
    const isAlAnswerSourceOption = inputArr.every((val) => this.answerSource.includes(val))
    const isAllDifferentValues = inputArr.every((val, index) => inputArr.indexOf(val) === index)
    return isLengthValid && isAlAnswerSourceOption && isAllDifferentValues
  }

  private getAnswerLength() {
    switch(this.mode)
    {
      case "normal":
        return 3
      case 'hard':
        return 4
      default:
        throw new Error(`${this.mode} は無効なモードです。`)
    }
  }
}

(async () => {
  const hitAndBlow = new HitAndBlow()
  await hitAndBlow.setting()
  await hitAndBlow.play()
  //await hitAndBlow.end()
})()


```


```shell
$ npm run dev
$ npm run start
```

