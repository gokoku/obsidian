#js/react 

---
2021-08-25

# React.createElement を使ってレンダリング

```shell
$ npx create-react-app component
$ cd component
$ yarn start
```


index.js

```js
import React from 'react'
import ReactDOM from 'react-dom'

const items = [
  '1 cup unsallted butter',
  '1 cup crunchy peanut butter',
  '1 cup brown sugar',
  '1 cup white sugar',
  '2 eggs',
  '2.5 cups all purpose flour',
  '0.5 teapoon salt',
]

const IngredientsList = (props) =>
  React.createElement(
    'ul',
    { className: 'ingredients' },
    props.items.map((Ingredient, i) =>
      React.createElement('li', { key: i }, Ingredient),
    ),
  )

ReactDOM.render(
  React.createElement(IngredientsList, { items }, null),
  document.getElementById('root'),
)
```

![[Pasted image 20210825220219.png]]

これが JSX を使わない素の React らしい。

