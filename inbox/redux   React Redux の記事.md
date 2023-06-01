#js/react 

---
2021-12-13

# React Redux

https://medium.com/@qdangdo/react-redux-a43176f65ade

カウンターだけ抜粋。

```shell
$ npx create-react-app learn-redux

$ cd learn-redux
$ npm install redux
$ npm install react-redux

```

src/components/counter/Counter.js

View でボタンクリックと dispatch (action) をバインド。

ここで、useSelector, useDispatch は React Hook 。

useSelector は、
* store の state を presentational comonent へ持ってくる。
* action がdispatch されると実行される。

useDispatch
* action をdispatch するために使用。
* store を変更しない限り、返り値のdispatch関数は変更されない。

connect 関数の代わりに使うと、見通しが俄然良くなるらしい。

https://tech.stmn.co.jp/entry/2020/10/29/161055

```js
import { useSelector, useDispatch } from 'react-redux'
import { increment, decrement } from '../../states/counter'

const Counter = () => {
  const counter = useSelector((state) => state.counter)
  const dispatch = useDispatch()

  return (
    <div>
      <h1>Counter {counter}</h1>
      <button onClick={() => dispatch(increment(5))}>+</button>
      <button onClick={() => dispatch(decrement(5))}>-</button>
    </div>
  )
}

export default Counter

```

src/states/counter.js

```js
export const increment = (num) => {
  return {
    type: 'INCREMENT',
    payload: num,
  }
}

export const decrement = (num) => {
  return {
    type: 'DECREMENT',
    payload: num,
  }
}

const counterReducer = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + action.payload
    case 'DECREMENT':
      return state - action.payload
    default:
      return state
  }
}

export default counterReducer

```

src/states/allReducers.js
本当はここで複数の Reducer を纏めてるらしい。

```js
import counterReducer from './counter'
//import loggerReducer from './logger'
//import friendsListReducer from './friends'
import { combineReducers } from 'redux'

const allReducers = combineReducers({
  counter: counterReducer,
  //isLogged: loggerReducer,
  //friends: friendsListReducer,
})

export default allReducers
```

src/index.js

```js

import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import reportWebVitals from './reportWebVitals'
import { createStore } from 'redux'
import { Provider } from 'react-redux'
import allReducers from './states/allReducers'

const store = createStore(
  allReducers,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__(),
)

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root'),
)

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals()

```


src/App.js

```js
import Counter from './components/counter/Counter'

function App() {
  return (
    <div className='App'>
      <Counter />
    </div>
  )
}

export default App

```


## わかり易いアニメーション

Store の中に Reducer がある。

![](https://miro.medium.com/max/700/0*Grai0oXEgg3EVVt_.gif)


