#ts #js

---
2021-12-06

# Promise でモーダル

https://ics.media/entry/211203/


```shell
$ npm init vite@latest promise-modal
$ cd promise-modal
$ npm i
$ npm i sass
$ npm dev run
```
 
 vanila-ts で。
 
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
        font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
        padding: 20px;
        color: #444;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <h1>demo1. Promiseでモーダル</h1>
      <p>
        Promiseを使用して独自のモーダルを標準のalert/confirmと同様に簡単に使えるようにしたサンプルです。
      </p>
      <button id="buttonShowModal">Show Modal</button>
    </div>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>
```

style.scss

```scss
.ModalAlert {
  display: flex;
  position: fixed;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
  overflow: hidden;
  background-color: #00000033;
  backdrop-filter: blur(8px);
  > div {
    width: min(60%, 500px);
    border: 1px solid #a;
    background-color: #f;
    padding: 12px 24px;
    text-align: center;
    box-shadow: 0 0 20px #00000033;
  }
  p {
    text-align: left;
  }
  button {
    display: inline-block;
    min-height: 32px;
    border: none;
    background-color: rgb(90, 70, 120);
    color: #f;
    margin-right: 4px;
    border-radius: 4px;
    padding: 4px 24px;
  }
}
```

src/modal.ts

```ts
const createConfirm = (
  message: string,
  onClose: (answer: boolean) => void,
  captionYes: string,
  captionNo: string
) => {
  const outer = document.createElement('div')
  const modal = document.createElement('div')
  const text = document.createElement('p')
  const buttonYes = document.createElement('button')
  const buttonNo = document.createElement('button')

  outer.className = "ModalAlert"
  text.textContent = message
  buttonYes.textContent = captionYes
  buttonNo.textContent = captionNo ?? ''

  const close = (answer: boolean) => {
    document.body.removeChild(outer)
    onClose(answer)
  }

  buttonYes.addEventListener('click', () => close(true))
  buttonNo.addEventListener('click', () => close(false))
  modal.appendChild(text)
  if (captionNo) {
    modal.appendChild(buttonNo)
  }
  modal.appendChild(buttonYes)
  outer.appendChild(modal)
  document.body.appendChild(outer)
}

export const modalConfirm = async (
  message: string,
  captionYes: "はい",
  captionNo?: "いいえ"
): Promise<boolean> => {
  return new Promise((resolve) => {
    createConfirm(message, resolve, captionYes, captionNo)
  })
}

export const modalAlert = async (
  message: string,
  captionOK: "はい",
): Promise<void> => {
  return new Promise((resolve) => {
    createConfirm(message, resolve, captionOK)
  })
}
```

src / main.ts

```ts
import './style.scss'
import { modalAlert, modalConfirm} from './modal'

const sampleShowModal = async () => {
  const isLikeDog = await modalConfirm("犬と猫どっちが好き?", "犬", "猫")
  const animal = isLikeDog ? "犬" : "猫"

  const isLikeCoffee = await modalConfirm("コーヒーとカフェどっちが好き?", "コーヒー", "カフェ")
  const drink = isLikeCoffee ? "コーヒー" : "カフェ"

  await modalAlert(`あなたが好きな動物は${animal}です。`, "OK");
  await modalAlert(`あなたが好きな飲み物は${drink}です。`, "OK");
}

document.getElementById('buttonShowModal').addEventListener('click', sampleShowModal)
```

