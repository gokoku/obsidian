#js/react 

---
2021-09-02

# Create react app を使わないで組む
「React ハンズオンラーニング」p93~

```shell
$ mkdir recipes-app
$ cd recipes-app
$ npm init -y
$ npm install react react-dom serve
```

```
├── data
|    └── recipes.json
├── node_modules
├── package-lock.json
├── package.json
├── index.html
├── src
    ├── index.js
    ├── components
    ├── Ingredient.js
    ├── IngredientsList.js
    ├── Instructions.js
    ├── Menu.css
    ├── Menu.js
    └── Recipe.js

```

src/components/Instructions.js
```js
import React from 'react'

export default function Instructions({ title, steps }) {
  return (
    <section className='instructions>'>
      <h2>{title}</h2>
      {steps.map((step, i) => (
        <p key={i}>{step}</p>
      ))}
    </section>
  )
}
```

src/components/Ingredient.js
```js
import React from 'react'

export default function Ingredient({ amount, measurement, name }) {
  return (
    <li>
      {amount} {measurement} {name}
    </li>
  )
}
```

src/components/IngredientsList.js
```js
import React from 'react'
import Ingredient from './Ingredient'

export default function IngredientsList({ list }) {
  return (
    <ul className='ingredients'>
      {list.map((ingredient, i) => (
        <Ingredient key={i} {...ingredient} />
      ))}
    </ul>
  )
}
```

src/components/Recipe.js
```js
import React from 'react'
import Instructions from './Instructions'
import IngredientsList from './IngredientsList'

export default function Recipe({ name, ingredients, steps }) {
  return (
    <section id={name.toLowerCase().replace(/ /g, '-')}>
      <h1>{name}</h1>
      <IngredientsList list={ingredients} />
    </section>
  )
}

```

src/components/Menu.js
```js
import Recipe from './Recipe'

function Menu({ recipes }) {
  return (
    <article>
      <header>
        <h1>Delicious Recipes</h1>
      </header>
      <div className='recipes'>
        {recipes.map((recipe, i) => (
          <Recipe key={i} {...recipe} />
        ))}
      </div>
    </article>
  )
}

export default Menu
```

src/index.js
```js
import React from 'react'
import { render } from 'react-dom'
import data from '../data/recipes.json'
import Menu from './components/Menu'

render(<Menu recipes={data} />, document.getElementById('root'))
```

data/recipes.json
```js
[
  {
    "name": "Baked Salmon",
    "ingredients": [
      { "name": "Salmon", "amount": 1, "measurement": "l lb"},
      { "name": "Pine Nuts", "amount": 1, "measurement": "cup"},
      { "name": "Butter Lettuce", "amount": 2, "measurement": "cup"},
      { "name": "Yellow Squash", "amount": 1, "measurement": "med"},
      { "name": "Olive Oil", "amount": 0.5, "measurement": "cup"},
      { "name": "Garlic", "amount": 3, "measurement": "cloves"}
    ],
    "steps": [
      "Preheat the over to 350 degrees.",
      "Spread the olive oil around a glass baking dish.",
      "Add the salmon, garlic, and pine nuts to the dish.",
      "Bake rot 15 minutes",
      "Add the yellow squash and put back in the oven for 30 mins",
      "Remove from oven and let cool for 15 minutes. Add the lettuce and serve."
    ]
  },
  {
    "name": "Fish Tacos",
    "ingredients": [
      {"name": "Whitefish", "amount":1, "measurement": "l lb"},
      {"name": "Cheese", "amount":1, "measurement": "cup"},
      {"name": "Iceberg Lettuce", "amount":2, "measurement": "cups"},
      {"name": "Tomatoes", "amount":2, "measurement": "large"},
      {"name": "Tortillas", "amount":3, "measurement": "med"}
    ],
    "steps": [
      "Cook the fish on the grill until hot.",
      "Place the fish on the 3 tortillas.",
      "Top them with lettuce, tomatoes, and cheese"
    ]
  }
]
```


## ビルド環境の構築
```shell
$ npm install --save-dev webpack webpack-cli

Babel系
$ npm install babel-loader @babel/core --save-dev

$ npm install @babel/preset-env @babel/preset-react --save-dev
```

webpack.config.js
```js
var path = require('path')

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.join(__dirname, 'dist', 'assets'),
    filename: 'bundle.js',
  },
  module: {
    rules: [{ test: /\.js$/, exclude: /node_modules/, loader: 'babel-loader' }],
  },
	
  // ソースマップ
  devtool: "source-map",
}
```

.babelrc
```json
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

バンドルビルド
```shell
$ npx webpack --mode development
```

package.json
```json
"scripts": {
  "build": "webpack --mode production",
}
```



### バンドルファイルのロード

dist/index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title>React Recipes App</title>
</head>
<body>
  <div id="root"></div>
  <script src="assets/bundle.js"></script>
</body>
</html>
```

## 起動する
package.json
```json
"scripts": {
    "serve": "serve ./dist"
}
```

```shell
$ npx webpack --mode development

$ npm run serve
```


![[Pasted image 20210902105529.png]]