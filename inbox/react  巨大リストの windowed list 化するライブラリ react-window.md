#js/react 

---
2021-09-03

# 巨大リストの windowed list 化するライブラリ react-window

無限スクロールってやつか。

ちょこっとづつ取ってきて入れ替えながらスクロール表示する react-window

App.jsx
```jsx
import React from 'react'
import { FixedSizeList } from 'react-window'
import faker from 'faker'
import './App.css'

const bigList = [...Array(5000)].map(() => ({
  name: faker.name.findName(),
  email: faker.internet.email(),
  avatar: faker.internet.avatar()
}))


function App() {
  const renderRow = ({ index, style }) => (
    <div style={{ ...style, ... {display: "flex"}}} >
      <img
        src={bigList[index].avatar}
        alt={bigList[index].name}
        width={50}
        />
      <p>
        {bigList[index].name} - {bigList[index].email}
      </p>
    </div>
  )
  return (
    <FixedSizeList
    height={window.innerHeight}
    width={window.innerWidth - 20}
    itemCount={bigList.length}
    itemSize={50}
    >
      {renderRow}
    </FixedSizeList>
  )
}

export default App
```


![[Pasted image 20210903153742.png]]

凄すぎ!!!

