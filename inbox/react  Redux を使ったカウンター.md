#js/react #js/redux 

---
2021-08-02

# Redux を使ったカウンター

```shell
$ npx create-react-app myapp
$ cd myapp

$ npm install redux react-redux
```

* Action と Reducer を作成
	* 「add_count」Action を 「counter」Reducer 関数に渡すと処理してくれる

* Store の作成
	* createStore 関数を使って作成
	* 引数に Reducer 関数やミドルウェア拡張機能を渡す。

* Redux と React をつなぎこむ
	* Provider コンポーネントで挟むと Redux の Store が適応される
	* Redux に Action を Dispatch 関数で伝えるのに useDispatch フックを使う

* Redux から状態を参照する
	* useSelector フックを使って抽出する


### index.js

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import App from './App.js'

// 初期 state を設定する
const initialState = { count: 0 }
// Action 用の Reducer を作る
const counter = (state = initialState, action) => {
  switch (action.type) {
    case 'add_count':
      const newCount = state.count + 1
      return {
        count: newCount,
      }
    default:
      return state
  }
}

// createStore() に作成した Reducer を渡す
const store = createStore(counter)

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root'),
)

```

### App.js

```js
import { useDispatch, useSelector } from 'react-redux'

function App() {
  // react-redux より Dispatch を行うフックを呼び出す
  const dispatch = useDispatch()
  // react-reeux より useSelector のフックでstate を抽出する
  const count = useSelector((state) => state.count)

  const handleAddCount = () => {
    dispatch({ type: 'add_count' })
  }

  return (
    <div className='App'>
      <button onClick={handleAddCount}>+1</button>
      <p>{count}</p>
    </div>
  )
}

export default App

```

![[Pasted image 20210802132454.png]]

# useReducer フックを使ったカウンター

useReducer フックを使えば、もっと手軽に Reducer を使うことが出来る。

### index.js

```js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App.js'

ReactDOM.render(<App />, document.getElementById('root'))
```

### App.js

```js
import { useReducer } from 'react'

const initialState = 0

const reducer = (count = initialState, action) => {
  switch (action.type) {
    case 'add_count':
      const newCount = count + 1
      return newCount
    default:
      return count
  }
}

function App() {
  // useReducer をつかって state と Dispatch関数を呼び出す
  const [count, dispatch] = useReducer(reducer, initialState)
  // Dispatch関数を呼び出すイベント
  const handleAddCount = () => {
    dispatch({ type: 'add_count', payload: count })
  }

  return (
    <div className='App'>
      <button onClick={handleAddCount}>+1</button>
      <p>{count}</p>
    </div>
  )
}

export default App
```

![[Pasted image 20210802135221.png]]

# Redux と useReducer の違い

## Redux

グローバルで状態を管理し、コンポーネントを横断する。

![[Pasted image 20210802140220.png]]


## useReducer

コンポーネントの中だけで状態が完結する。

![[Pasted image 20210802140401.png]]
