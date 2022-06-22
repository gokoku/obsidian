#ts #js

---
2021-12-06

# Promiseで同時処理数をコントロールする


なんと、Promise を個数制限する。

結構難しいことになってる。

これは凄いよ!!

https://ics.media/entry/211203/


```shell
$ npm init vite@latest promise-modal
$ cd promise-modal
$ npm i
$ npm i sass
$ npm dev run
```


index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
    <style>
      body {
        font-family: sans-serif;
        padding: 20px
        color: #444;
      }
      h1 {
        color: rgb(90, 70, 120)
      }
      p {
        text-align: center;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <div id="imageWall"></div>
    </div>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>

```


src / sleep.ts

デモ用のウェイト

```ts
export const sleep = async (millisec: number) =>
  new Promise((resolve) => setTimeout(resolve, millisec))
```

src / lock.ts

```ts
import { sleep } from "./sleep"

const MAX_ROOMS = 3

type Releaser = () => void
type Resolve = (releaser: Releaser) => void

// 使用中の部屋のリスト　キーは Symbol
let rooms: Symbol[] = []
// 処理待ちのリスト 待ってるPromise の resolve関数を保持しておく
const waitingList: Resolve[] = []

/**
 * ロックを取得、ロックを解除する処理を持つ Promise を返す
 * @returns Promise<Releaser> ロック解放関数。処理が終わったら解放すること
 */
export const enter = () => {
  const promise = new Promise<Releaser>(resolve => {
    // Promise resolve関数を待ち行列に追加
    waitingList.push(resolve)
  })
  // 処理の開始
  tryNext()
  // 開始できたか、待たされているかにかかわらず、Promiseを返す
  return promise
}

/**
 * ロックを解除する
 * @param room 割り当てられた部屋のキー
 */
const release = (room: Symbol) => {
  rooms = rooms.filter(r => r !== room)
  tryNext() // まってる処理があれば開始
}

/**
 * 部屋の空きがあり、待ちリストに待機している処理があれば、部屋を割り当てて開始する。
 */
const tryNext = () => {
  // 上限オーバーなら何もしない
  if (rooms.length >= MAX_ROOMS) return
  // 待ちリストが空なら何もしない
  const resolve = waitingList.shift()
  if (!resolve) return

  // 新しい部屋を作成
  const room = Symbol()
  rooms.push(room)
  // ロックを解除する関数を返して Promise を resolve
  // resolve するまでPromiseは待つ?
  // としたらここを通すコントロールで個数制限をかけてる?
  resolve(() => release(room))
}
```


src / main.ts

```ts
import './style.scss'
import { enter } from "./lock"
import { sleep } from "./sleep"


/*
  Demo3:
  このサンプルではPromiseを使って「同時処理数を制限する仕組み」を作り、それを使って画像の大量読み込みを制御します。
  画面ロード時に36枚の画像の読み込みが要求されますが、同時処理数が3に制限されているため同時にダウンロードされるのは常に3枚以下になります。
  この仕組みはAPI呼び出し・ファイルアクセス・モーダル等のUI制御など、さまざまな場面で応用できます。
*/

const parent = document.getElementById('imageWall')

/**
 * 画像の読み込みを行う
 * 大量に呼び出される場合、同時処理数が制限される
 * @param id 読みこむ画像のID
 */
const loadImage = async (id: number) => {
  const el = document.createElement('div')
  parent.appendChild(el)

  // ロック獲得 -- 処理数が上限を超えている場合、ここで待たされる
  const release = await enter()

  // ローディング画像を表示
  el.classList.add("loading")
  //await sleep(Math.random() * 500) // デモ用にランダムな時間待機
  //await sleep(5000)

  // 画像を読み込み完了・失敗時のイベントハンドラを設定
  const onload = () => {
    el.classList.remove("loading")
    release() // ロック解放
  }

  // img要素を作成して、src属性に画像のURLを設定
  const img = new Image()
  img.onload = img.onabort = onload
  img.src = `https://picsum.photos/seed/${id}/300/300`
  el.appendChild(img)
}

// 36枚の画像を読み込む
for (let index = 0; index < 36; index++) {
  loadImage(index + 1)
}
```

