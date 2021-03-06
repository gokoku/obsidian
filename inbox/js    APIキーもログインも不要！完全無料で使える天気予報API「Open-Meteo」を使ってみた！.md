#js #web

---
2021-11-05

# [APIキーもログインも不要！完全無料で使える天気予報API「Open-Meteo」を使ってみた！]

記事

https://paiza.hatenablog.com/entry/2021/11/04/130000

Open-Meteo Home page

https://open-meteo.com/en


## Weather Forecast API  URL Builder

https://open-meteo.com/en/docs



### js

現在地 : 39.788282684554616, 141.14521062704097


```
https://api.open-meteo.com/v1/forecast?latitude=39.7882&longitude=141.1452&hourlyf=temperature_2m&timezone=Asia%2FTokyo
```

```shell
$ npm init vite@latest open-meteo
$ cd open-meteo
$ npm i
$ npm run def
```

typeScript でやってみた。

```ts
import './style.css'

const app = document.querySelector<HTMLDivElement>('#p')!
const res = await fetch('https://api.open-meteo.com/v1/forecast?latitude=39.7882&longitude=141.1452&hourlyf=temperature_2m&timezone=Asia%2FTokyo')
const data = await res.json()
console.log(data)
app.innerHTML = `${JSON.stringify(data)}`
```

js 互換なんだな。ts にするのはおいおいってとこでいいな。

![[Pasted image 20211105101907.png]]

#### 最高気温、最低気温をグラフ

URL-Builder で最低最高気温データのAPI を作る。

1週刊のデイリーのデータの指定。

![[Pasted image 20211105102336.png]]

```
https://api.open-meteo.com/v1/forecast?latitude=39.7882&longitude=141.1452&daily=temperature_2m_max,temperature_2m_min&timezone=Asia%2FTokyo
```

main.ts

```ts
const app = document.querySelector<HTMLDivElement>('#p')!
const res = await fetch('https://api.open-meteo.com/v1/forecast?latitude=39.7882&longitude=141.1452&daily=temperature_2m_max,temperature_2m_min&timezone=Asia%2FTokyo')
const data = await res.json()
console.log(data)
app.innerHTML = `${JSON.stringify(data)}`
```

![[Pasted image 20211105102636.png]]

これをグラフにするのか。

```shell
$ npm i chart.js
```

import Chart from "chart.js/auto" でうまくいく。

main.ts

```js
import './style.css'
import Chart from "chart.js/auto"

const app = document.querySelector<HTMLCanvasElement>('#p')!
const res = await fetch('https://api.open-meteo.com/v1/forecast?latitude=39.7882&longitude=141.1452&daily=temperature_2m_max,temperature_2m_min&timezone=Asia%2FTokyo')
const json = await res.json()
drawChart(json)

function drawChart(json: any): void {
  const mydata = {
    labels: json.daily.time,
    datasets: [{
      label: '最高気温',
      data: json.daily.temperature_2m_max,
      borderColor: 'rgb(192, 75, 75)',
    },{
      label: '最低気温',
      data: json.daily.temperature_2m_min,
      borderColor: 'rgb(75, 75, 192)',
    }]
  }
  new Chart(app, {
    type: 'line',
    data: mydata,
  });
}
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
  </head>
  <body>
    <canvas id="app"></canvas>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>

```

![[Pasted image 20211105111810.png]]


####  これでもいいみたい

typescript は普通に async な感じなのかな。

なので、await res.json() でデータを取った後はそのまま同期的に書いても動いた。

ところで、ほぼ copilot 君に書いてもらった。というものすごさ。

こんな優秀な相棒のお陰で、僕は相当勉強になるよ。


```js
import './style.css'
import Chart from "chart.js/auto"

const app = document.querySelector<HTMLCanvasElement>('#p')!
const res = await fetch('https://api.open-meteo.com/v1/forecast?latitude=39.7882&longitude=141.1452&daily=temperature_2m_max,temperature_2m_min&timezone=Asia%2FTokyo')
const json = await res.json()

const mydata = {
  labels: json.daily.time,
  datasets: [{
    label: '最高気温',
    data: json.daily.temperature_2m_max,
    backgroundColor: 'rgba(255, 99, 132, 0.2)',
    borderColor: 'rgba(255, 99, 132, 1)',
    borderWidth: 1
  }, {
    label: '最低気温',
    data: json.daily.temperature_2m_min,
    backgroundColor: 'rgba(54, 162, 235, 0.2)',
    borderColor: 'rgba(54, 162, 235, 1)',
    borderWidth: 1
  }]
}

new Chart(app, {
  type: 'line',
  data: mydata,
})
```

![[Pasted image 20211105113825.png]]


これの Android Appli 作ってみる?!
