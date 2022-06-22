#js/react

---
2021-08-02

# Context API を使った状態管理の例

React にある Context API を使った状態を共有する方法。

コンポーネント単位で状態を共有出来る。

Redux よりハードルが低くわかりやすいので、小~中規模ならこっちがいいらしい。

* Context API の Provider コンポーネントでラップする
* useContext フックで親コンポーネントの state を子コンポーネントに渡している

### index.js

```js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App.js'

ReactDOM.render(<App />, document.getElementById('root'))
```

### App.js

```js
import { useState, createContext, useContext } from 'react'

const Context = createContext({
  count: 0,
})

function App() {
  // カウンターのローカル state
  const [count, setCount] = useState(0)
  // カウンターの数値を更新する関数
  const handleAddCount = () => {
    setCount((preCounter) => preCounter + 1)
  }
  return (
    <Context.Provider value={{ count }}>
      <button onClick={handleAddCount}>+1</button>
      <ChildComponent />
    </Context.Provider>
  )
}

const ChildComponent = () => {
  const { count } = useContext(Context)
  return <p>count の数値は{count}です。</p>
}
export default App
```

![[Pasted image 20210802134237.png]]

