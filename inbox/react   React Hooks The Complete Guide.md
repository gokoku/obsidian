#js/react 

---
2021-08-24

# React Hooks: The Complete Guide

https://javascript.plainenglish.io/react-hooks-the-complete-guide-5c176ca825f6

オブジェクト指向の代わりに関数型アプローチを使ってコンポーネントを作ることが出来る。

![](https://miro.medium.com/max/700/1*_rLrdzMCsdQZS7D9Zoe4iA.jpeg)

## State Hook — useState()



## Effect Hook — useEffect()



## useRef()



## useContext()


## useCallback()



## useMemo()


## useReducer()


```js
import { useReducer } from 'react'

const initialCount = { count: 0 }

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 }
    case 'decrement':
      return { count: state.count - 1 }
    default:
      throw new Error()
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialCount)
  return (
    <div className='App'>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
    </div>
  )
}

export default Counter
```



## useImperativeHandle()


## useLayoutEffect()



## useDebugValue()


