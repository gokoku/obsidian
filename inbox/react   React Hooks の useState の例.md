#js/react

---
2021-08-02

# useState の使い方

```shell
$ npx create-react-app myapp
$ cd myapp
$ yarn start
```

useState から、state 値と、それを変更する関数が受け取れる。配列であるのが注意点。

上から下への受け渡しで、上の階層で useState() を使うと、同期出来る。

App.js

```js
import { useState } from 'react'

function Counter(props) {
  const { count, setCount } = props
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Add</button>
      クリック数: {count} 回
    </div>
  )
}

function App() {
  const [count, setCount] = useState(0)

  return (
    <div className='App'>
      <Counter count={count} setCount={setCount} />
      <Counter count={count} setCount={setCount} />
      <Counter count={count} setCount={setCount} />
    </div>
  )
}

export default App

```

![[Pasted image 20210802105810.png]]

各コンポーネントで useState() を使うと、みなバラバラな state 値が持てる。

```js
import { useState } from 'react'

function Counter(props) {
  const [count, setCount] = useState(0)
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Add</button>
      クリック数: {count} 回
    </div>
  )
}

function App() {
  return (
    <div className='App'>
      <Counter />
      <Counter />
      <Counter />
    </div>
  )
}

export default App
```

