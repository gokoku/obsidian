#js/react 

---
2021-07-27

# **15 Custom Hooks to Make your React Component Lightweight**

https://javascript.plainenglish.io/15-custom-hooks-to-make-your-react-component-lightweight-8b59b122d83a

![](https://miro.medium.com/max/1400/1*DKDNvL6XMQkI_55M7WaImA.jpeg)

## 2. useInterval

This hook to use for interval-related functionalities. Which handles `clearInterval`on component unmount automatically. It also allows pausing the interval by setting the delay to null.

clearInterbal のハンドリングをコンポーネントのアンマウントで自動的にやってくれる?
delay の値を null で一時停止する?

```shell
$ npx create-react-app use-interval

$ yarn add react-use

$ yarn start
```

src/index.js

```js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import Demo from './UseInterval'

import reportWebVitals from './reportWebVitals'

ReactDOM.render(
  <React.StrictMode>
    <Demo />
  </React.StrictMode>,
  document.getElementById('root'),
)

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals()

```


src/UseInterval.js

```js
import * as React from 'react'
import { useInterval, useBoolean } from 'react-use'
import './UseInterval.css'

const Demo = () => {
  const [count, setCount] = React.useState(0)
  const [delay, setDelay] = React.useState(1000)
  const [isRunning, toggleIsRunning] = useBoolean(true)

  useInterval(
    () => {
      setCount(count + 1)
    },
    isRunning ? delay : null,
  )

  const clearCount = () => {
    setCount(0)
  }

  return (
    <div className='container'>
      <div className='delayInput'>
        <p>Delay</p>
        <input
          value={delay}
          onChange={(event) => setDelay(Number(event.target.value))}
        />
        <p>msec</p>
      </div>
      <h1>count: {count}</h1>
      <div className='buttons'>
        <button className='button' onClick={toggleIsRunning}>
          {isRunning ? 'Stop' : 'Start'}
        </button>
        <button className='button' onClick={clearCount}>
          clear
        </button>
      </div>
    </div>
  )
}

export default Demo

```

src/UseInterval.css

```css
.container {
  text-align: center;
  margin: 2rem auto;
  width: 30%;
}

.delayInput {
  display: flex;
  padding: 0.2
  rem;
  justify-content: space-around;
  align-items: center;
  background-color: [[5bb966]];
  color: white;
  border-radius: 0.5rem;
}
.delayInput p {
  font-weight: bold;
  font-size: 1rem;
}
.delayInput input {
  border-radius: 0.5rem;
  border: none;
  padding: 0.5rem;
  text-align: right;
}
.container .buttons {
  display: flex;
  justify-content: center;
  gap: 2rem;
}
.container .button {
  width: 100px;
  height: 30px;
  border: none;
  color: white;
  font-weight: bold;
  border-radius: 0.5rem;
  background-image:
  radial-gradient(at top, [[1ecff7]], transparent),
  radial-gradient(at bottom, [[1e7fc1]], transparent);
  box-shadow: 0 8px 8px 0 rgba(31,38,135,0.2);
  cursor: pointer;
}
.container .button:hover {
  background-image:
    radial-gradient(at top, [[32f977]], transparent),
    radial-gradient(at bottom, [[1e7fc1]], transparent);
}
```


![[Pasted image 20210727104516.png]]