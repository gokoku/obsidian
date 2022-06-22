#ts #js

---
2021-12-06

# Promise で API を確実にキャッシュして通信を効率的にする

https://ics.media/entry/211203/

```shell
$ npm init vite@latest promise-modal
$ cd promise-modal
$ npm i
$ npm i sass
$ npm dev run
```

同じjsonファイルを使ってる場合、Promise をキャッシュに使って無駄な通信をしないようにする。

こりゃすごいや。

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
        margin: 0;
        padding: 20px;
        color: #444;
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
      }
      h1 {
        color: rgb(90, 70, 120);
      }
      #i {
        display: flex;
        gap: 4px;
        flex-wrap: wrap;
        justify-content: center;
        margin-bottom: 30px;
      }
      .component {
        width: 28%;
        min-width: 300px;
        border: 1px solid #a;
        padding: 4px;
        display: flex;
        flex-direction: column;
        text-align: center;
        background-color: #f;
        border-radius: 4px;
      }
      .component .title {
        font-weight: bold;
        border-bottom: 1px solid #a;
      }
      .component .value {
        min-height: 80px;
        display: flex;
        align-items: center;
        justify-content: center;
      }
      .notice {
        color: gray;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <h1>demo2. PromiseでAPIキャッシュを制御</h1>
      <p>
        東京都の今日の天気・風・波の強さです
      </p>
      <div id="api">
        <div class="component weather"><div class="title">天気</div><div class="value"></div></div>
        <div class="component wind"><div class="title">風</div><div class="value"></div></div>
        <div class="component wave"><div class="title">波</div><div class="value"></div></div>
      </div>

    </div>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>
```

src / api.ts

```ts
const API_ENDPOINT =
  "https://www.jma.go.jp/bosai/forecast/data/forecast/130000.json"; // 東京
const CACHE_LEFETIME_SEC = 3600

let dataPromise: Promise<any> | undefined
let expired = Date.now()

const loadWeatherData = () => {
  const now = Date.now()
  if (now > expired) {
    dataPromise = undefined
  }
  if (dataPromise) {
    return dataPromise
  }
  expired = now + CACHE_LEFETIME_SEC * 1000
  dataPromise = fetch(API_ENDPOINT).then((res) => res.json())
  return dataPromise
}

export const getTokyoWeather = async () => {
  const data = await loadWeatherData()
  return data?.[0]?.timeSeries[0]?.areas?.[0]?.weathers?.[0] ?? ""
}

export const getTokyoWind  = async () => {
  const data = await loadWeatherData()
  return data?.[0]?.timeSeries[0]?.areas?.[0]?.winds?.[0] ?? ""
}

export const getTokyoWave = async () => {
  const data = await loadWeatherData()
  return data?.[0]?.timeSeries[0]?.areas?.[0]?.waves?.[0] ?? ""
}
```


src / main.ts

```ts
import './style.css'
import { getTokyoWave, getTokyoWeather, getTokyoWind } from './api'

const showTokyoWeather = async () => {
  const el = document.querySelector('.weather .value')
  if (!el) return
  el.textContent = 'Loading...'
  el.textContent = (await getTokyoWeather()) ?? "取得失敗"
  await console.log(el.textContent)
}

const showTokyoWind = async () => {
  const el = document.querySelector('.wind .value')
  if (!el) return
  el.textContent = 'Loading...'
  el.textContent = (await getTokyoWind()) ?? "取得失敗"
  console.log(el.textContent)
}

const showTokyoWave = async () => {
  const el = document.querySelector('.wave .value')
  if (!el) return
  el.textContent = 'Loading...'
  el.textContent = (await getTokyoWave()) ?? "取得失敗"
  console.log(el.textContent)
}

showTokyoWeather()
showTokyoWind()
showTokyoWave()
```


