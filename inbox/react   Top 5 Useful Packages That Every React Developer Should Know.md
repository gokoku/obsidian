#js/react 

---
2021-09-13

# Top 5 Useful Packages That Every React Developer Should Know

https://devdojo.com/zahabkakar/top-5-useful-packages-that-every-react-developer-should-know

```shell
$ npx create-react-app my-app
$ cd my-app
```

## Axios
```shell
$ yarn add axios
```

Axios.js

```js
import axios from 'axios'
import './App.css'

function Axios() {
  const axiosInstance = axios.create({
    baseUrl: 'https://google.com',
  })
  axiosInstance
    .get('/')
    .then((response) => {
      console.log(response)
    })
    .catch((error) => {
      console.log(error)
    })
    .then(() => {
      //always executed
      console.log('OK!!!!!!!!')
    })
  return <div className='App'>Axios</div>
}

export default Axios
```

index.js
```js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import Axios from './Axios'

ReactDOM.render(
  <React.StrictMode>
    <App />
    <Axios />
  </React.StrictMode>,
  document.getElementById('root'),
)
```

## Reducer

```shell
$ yarn add redux
```

Redux.js

```js
function CounterReducer(state = { value: 0 }, action) {
  switch (action.type) {
    case 'counter/incremented':
      return { value: state.value + 1 }
    case 'counter/decremented':
      return { value: state.value - 1 }
    default:
      return state
  }
}

export default CounterReducer
```

index.js

```js
import React from 'react'
import ReactDOM from 'react-dom'
import CounterReducer from './Redux'
import { createStore } from 'redux'

let store = createStore(CounterReducer)
const plusHundler = () => store.dispatch({ type: 'counter/incremented' })
const minusHundler = () => store.dispatch({ type: 'counter/decremented' })

store.subscribe(() => console.log(store.getState()))

ReactDOM.render(
  <React.StrictMode>
    <div>
      <button onClick={plusHundler}>+</button>
      <button onClick={minusHundler}>-</button>
    </div>
  </React.StrictMode>,
  document.getElementById('root'),
)
```

## Formik

```shell
$ yarn add formik
```

SignupForm.js

```js
import React from 'react'
import { useFormik } from 'formik'

const SignupForm = () => {
  const formik = useFormik({
    initialValues: {
      email: '',
    },
    onSubmit: (values) => {
      alert(JSON.stringify(values, null, 2))
    },
  })
  return (
    <form onSubmit={formik.handleSubmit}>
      <label htmlFor='email'>Email Address</label>
      <input
        id='email'
        name='email'
        type='email'
        onChange={formik.handleChange}
        value={formik.values.email}
      />
      <button type='submit'>Submit</button>
    </form>
  )
}

export default SignupForm
```

index.js

```js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import SignupForm from './SignupForm'

ReactDOM.render(
  <React.StrictMode>
    <SignupForm />
  </React.StrictMode>,
  document.getElementById('root'),
)
```

![[Pasted image 20210913110702.png]]

## Styled Components

CSS tool

```shell
$ yarn add styled-components
```

MyComponent.js

```js
import styled from 'styled-components'

const Button = styled.button`
  background-color: black;
  font-size: 18px;
  color: white;
`
function MyComponent() {
  return <Button> Sign up </Button>
}

export default MyComponent

```

index.js

```js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import MyComponent from './MyComponent'

ReactDOM.render(
  <React.StrictMode>
    <MyComponent />
  </React.StrictMode>,
  document.getElementById('root'),
)

```

![[Pasted image 20210913111201.png]]

## Chakra UI
React component toolkit

```shell
$ yarn add '@chakra-ui/react' '@emotion/react@^11' '@emotion/styled@^11' 'framer-motion@^4'
```

効かない。後で。

