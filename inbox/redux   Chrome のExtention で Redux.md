#js/redux #chrome 

---
2021-08-02

# Chrome の Redux extention の使い方

ワーニングが出てたので、

```js
export const configureStore = () => {
  return createStore(
    cartReducer,
    compose(
      process.env.NODE_ENV === 'development' && window.devToolsExtension
        ? window.devToolsExtension()
        : (f) => f,
    ),
  )
}
```

devToolsExtension は非推奨で、こうしたらワーニングが消えた。

```js
export const configureStore = () => {
  return createStore(
    cartReducer,
    compose(
      process.env.NODE_ENV === 'development' &&
        window.__REDUX_DEVTOOLS_EXTENSION__
        ? window.__REDUX_DEVTOOLS_EXTENSION__()
        : (f) => f,
    ),
  )
}
```

window.__REDUX_DEVTOOLS_EXTENSION__

window.__REDUX_DEVTOOLS_EXTENSION__()

これでこうなる。

![[Pasted image 20210802172727.png]]

