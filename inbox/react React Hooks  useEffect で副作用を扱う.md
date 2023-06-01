#js/react

---
2021-08-02

# useEffect で副作用を扱う

```shell
$ npx create-react-app myapp
$ cd myapp
$ yarn start
```


App.js

```js
import { useEffect, useRef } from 'react'

function Pie(props) {
  const { percent, color = '[[ff00ff]]' } = props
  const canvasRef = useRef()

  useEffect(() => {
    const ctx = canvasRef.current.getContext('2d')
    ctx.clearRect(0, 0, 100, 100)
    ctx.fillStyle = color
    ctx.beginPath()
    ctx.moveTo(50, 50)
    ctx.arc(50, 50, 50, 0, (percent / 100) * Math.PI * 2, false)
    ctx.lineTo(50, 50)
    ctx.fill()
  })
  return <canvas ref={canvasRef} width={100} height={100} />
}

function App() {
  return (
    <div className='App'>
      <Pie percent={80} />
    </div>
  )
}

export default App

```

![[Pasted image 20210802111345.png]]

### input で動かしたい


```js
import { useState, useEffect, useRef } from 'react'

function Pie(props) {
  const { percent, color = '[[ff00ff]]' } = props
  const canvasRef = useRef()

  useEffect(() => {
    const ctx = canvasRef.current.getContext('2d')
    ctx.clearRect(0, 0, 100, 100)
    ctx.fillStyle = color
    ctx.beginPath()
    ctx.moveTo(50, 50)
    ctx.arc(50, 50, 50, 0, (percent / 100) * Math.PI * 2, false)
    ctx.lineTo(50, 50)
    ctx.fill()
  })
  return <canvas ref={canvasRef} width={100} height={100} />
}

function Slider(props) {
  const { dig, setDig } = props
  return (
    <>
      <input
        type='range'
        name='digree'
        min='0'
        max='100'
        value={dig}
        onChange={(e) => {
          const d = parseInt(e.target.value)
          console.log(d)
          setDig(d)
        }}
      />
      <p>{dig} %</p>
    </>
  )
}

function App() {
  const [dig, setDig] = useState(50)

  return (
    <div className='App'>
      <Slider dig={dig} setDig={setDig} />
      <Pie percent={dig} />
    </div>
  )
}

export default App
```

![[Pasted image 20210802113316.png]]

スゲーあっという間に出来てしまった。React 恐るべし。

この副作用とモナドは関係あるのだろうか。

