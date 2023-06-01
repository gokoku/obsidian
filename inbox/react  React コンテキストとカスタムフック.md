#js/react 

---
2021-09-02

# コンテキストとカスタムフック

なんだこりゃ!? 凄すぎ!!

絡まった感じが全くしない!!!

コンテキストの操作をカスタムフックで隠蔽。

コンテキストをコンシューマに公開せず、データ共有。


ColorProvider.js
```js
import React, { createContext, useState, useContext } from 'react'
import colorData from './color-data.json'
import { v4 } from 'uuid'

export const ColorContext = createContext()
export const useColors = () => useContext(ColorContext)

export default function ColorProvider({ children }) {
  const [colors, setColors] = useState(colorData)

  const addColor = (title, color) =>
    setColors([...colors, { id: v4(), rating: 0, title, color }])

  const rateColor = (id, rating) =>
    setColors(
      colors.map((color) => (color.id === id ? { ...color, rating } : color)),
    )

  const removeColor = (id) =>
    setColors(colors.filter((color) => color.id !== id))

  return (
    <ColorContext.Provider value={{ colors, addColor, removeColor, rateColor }}>
      {children}
    </ColorContext.Provider>
  )
}
```

これがカスタムブロバイダー。

これを共有するために 

index.js
```js
import React from 'react'
import ColorProvider from './ColorProvider'
import { render } from 'react-dom'
import './index.css'
import App from './App'

render(
  <ColorProvider>
    <App />
  </ColorProvider>,
  document.getElementById('root'),
)
```

App.js
```js
import React from 'react'
import ColorList from './ColorList.js'
import AddColorForm from './AddColorForm'

function App() {
  return (
    <>
      <AddColorForm />
      <ColorList />
    </>
  )
}

export default App
```

AddColorForm.js
```js
import React from 'react'
import { useInput } from './hooks'
import { useColors } from './ColorProvider'

export default function AddColorForm() {
  const [titleProps, resetTitle] = useInput('')
  const [colorProps, resetColor] = useInput('#000000')
  const { addColor } = useColors()

  const submit = (e) => {
    e.preventDefault()
    addColor(titleProps.value, colorProps.value)
    resetTitle()
    resetColor()
  }
  return (
    <form onSubmit={submit}>
      <input
        {...titleProps}
        type='text'
        placeholder='color title...'
        required
      />
      <input {...colorProps} type='color' required />
      <button>ADD</button>
    </form>
  )
}
```

AddColorForm で使う input のカスタムフック。

hooks.js
```js
import { useState } from 'react'

export const useInput = (initialValue) => {
  const [value, setValue] = useState(initialValue)
  return [
    { value, onChange: (e) => setValue(e.target.value) },
    () => setValue(initialValue),
  ]
}
```


ColorList.js
```js
import React from 'react'
import Color from './Color'
import { useColors } from './ColorProvider'

export default function ColorList() {
  const { colors } = useColors()
  return (
    <div>
      {colors.map((color) => (
        <Color key={color.id} {...color} />
      ))}
    </div>
  )
}
```

Color.js
```js
import StarRating from './StarRating'
import { useColors } from './ColorProvider'

export default function Color({ id, title, color, rating }) {
  const { rateColor, removeColor } = useColors()
  return (
    <section>
      <h1>{title}</h1>
      <button onClick={() => removeColor(id)}>X</button>
      <div style={{ height: 50, backgroundColor: color, color: 'white' }}>
        {color}
      </div>
      <StarRating
        selectedStars={rating}
        onRate={(rating) => rateColor(id, rating)}
      />
    </section>
  )
}
```

StarRating.js
```js
import React from 'react'
import { Star } from './Star.js'

export default function StarRating({
  totalStars = 5,
  selectedStars = 0,
  onRate = (f) => f,
}) {
  return (
    <>
      {[...Array(totalStars)].map((_, i) => (
        <Star
          key={i}
          selected={selectedStars > i}
          onSelect={() => onRate(i + 1)}
        />
      ))}
      <p>
        {selectedStars} of {totalStars} stars
      </p>
    </>
  )
}
```

Star.js
```js
import React from 'react'
import { FaStar } from 'react-icons/fa'

export const Star = ({ selected = false, onSelect = (f) => f }) => (
  <FaStar color={selected ? 'orange' : 'gray'} onClick={onSelect} />
)
```


これだけで、こんなアプリが出来る。

* スターをクリックすると変わる。
* 削除ボタンで削除出来る。
* 追加出来る。


![[Pasted image 20210902171006.png]]

