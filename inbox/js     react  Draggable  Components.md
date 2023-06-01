#js/react 


[Making Draggable Components in React](https://lo-victoria.com/making-draggable-components-in-react)

```shell
$ npx create-react-app my-app
$ cd my-app
$ npm install react-draggable
```

src/App.js

```js
import React, { useState } from 'react'
import './App.css'
import Draggable from 'react-draggable'

function App() {
  const [position, setPosition] = useState({ x: 0, y: 0 })

  const trackPos = (data) => {
    setPosition({ x: data.x, y: data.y })
  }

  return (
    <div className='App'>
      <Draggable onDrag={(e, data) => trackPos(data)}>
        <div className='box'>
          <div>Move me around!</div>
          <div>
            x: {position.x.toFixed(0)}, y: {position.y.toFixed(0)}
          </div>
        </div>
      </Draggable>
    </div>
  )
}

export default App
```

src/App.css

```js
body {
  background-color: [[282c34]];
}
.App {
  text-align: center;
}

.box {
  position: absolute;
  cursor: move;
  color: black;
  max-width: 215px;
  border-radius: 5px;
  padding: 1em;
  margin: auto;
  user-select: none;
  background-color: white;
}
```

たったこれだけでこんなことが出来てしまう。
ホワイトボードが簡単に作れるかも。
