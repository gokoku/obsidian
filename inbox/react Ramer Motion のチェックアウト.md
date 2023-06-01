#js/react 

---
2021-06-03

# Introduction to Framer Motion for React

https://blog.bitsrc.io/introduction-to-framer-motion-for-react-4ba5a9cabc1b

Framer Motion はアニメーションライブラリ using React

```shell
$ npx create-react-app framer-motion
$ cd framer-motion

$ npm install framer-motion
$ npm run start
```

src/App.js
```js
import './App.css'
import { motion } from 'framer-motion'

function App() {
  return (
    <motion.div
      style\={{ backgroundColor: 'orange', width: '30px', height: '30px' }}
      animate\={{ x: 100, scale: 1.5, rotate: 90 }}
    />
  )
}

export default App
```

reload すると動く。
![[Pasted image 20210603101307.png]]

```javascript
    <motion.div
      style={{ backgroundColor: 'orange', width: '30px', height: '30px' }}
      animate={{ y: [0, 150, 150, 0], rotate: 90 }}
      transition={{ duration: 3, repeat: Infinity }}
    />
/>
```
上下に行ったり来たりする。
![[Pasted image 20210603101617.png]]


```js
import './App.css'
import { motion } from 'framer-motion'

function App() {
  return (
    <motion.div
      style={{ backgroundColor: 'orange', padding: '30px', width: '100px' }}
      drag
      dragTransition={{
        min: 0,
        max: 100,
        bounceStiffness: 100,
      }}>
      Drag
    </motion.div>
  )
}

export default App

```

なんと、ドラッグできて、戻ってくる。

![[Pasted image 20210603103218.png]]

これは気持ちいいライブラリだ。
Vue にないのだろうか。



