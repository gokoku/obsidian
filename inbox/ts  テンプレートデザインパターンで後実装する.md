#ts

---
2022-03-15  08:59

# ts  テンプレートデザインパターンで後実装する

JSライブラリの TS 化をしてて、つまづいた後から上書きパターン。

TS ではクラス化すれば後乗せ実装が出来た。

[[ts  複数ファイルの import を解決するには webpack]]

想定Gameライブラリ

```ts:src/Game.ts
export abstract class Game {
  constructor () {
    this.flow()
  }

  abstract setup(): void
  abstract mainloop(): void
  flow(): void {
    this.setup()
    setInterval(() => {
      this.mainloop()
    },1000)
  }
}
```

実装ゲーム側で、setup と mainloop の中身を実装する形に出来る。

```ts:src/index.ts
import { Game } from './Game'

class MyGame extends Game {
  counter:number = 0
  constructor () {
    super();
  }
  setup(): void {
    console.log('setup');
  }
  mainloop(): void {
    console.log('mainloop: ' + this.counter)
    this.counter++
  }
}

new MyGame()
```

おー、オブジェクト指向だ。

```
$ node dist/index.js
setup
mainloop: 0
mainloop: 1
mainloop: 2
...
```

