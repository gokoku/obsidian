#js/react 

---
2021-06-17


https://material.io/

# Material-UI

https://codezine.jp/article/detail/14322?p=2

```shell
$ npx create-react-app material-ui
$ cd material-ui
$ yarn start
```

## Material-UI セットアップ

```shell
$ yarn add @material-ui/core
```

src/App.js

```js
import './App.css'
import Button from '@material-ui/core/Button'

function App() {
  return (
    <Button color='primary' variant='contained'>
      Hello World
    </Button>
  )
}

export default App
```

## Menu

```js
import { ThemeProvider, createMuiTheme } from '@material-ui/core/styles'
import lightBlue from '@material-ui/core/colors/lightBlue'
import teal from '@material-ui/core/colors/teal'
import AppBar from '@material-ui/core/AppBar'
import Button from '@material-ui/core/Button'
import Container from '@material-ui/core/Container'
import Toolbar from '@material-ui/core/Toolbar'
import Typography from '@material-ui/core/Typography'
import Box from '@material-ui/core/Box'
import Menu from '@material-ui/core/Menu'
import MenuItem from '@material-ui/core/MenuItem'
import { useState } from 'react'

const theme = createMuiTheme({
  palette: {
    primary: {
      main: lightBlue[800],
    },
    secondary: {
      main: teal[200],
    },
  },
})
function App() {
  return (
    <ThemeProvider theme={theme}>
      <AppBar position='static' color='primary'>
        <Toolbar>
          <Typography variant='h6'>Sample</Typography>
        </Toolbar>
      </AppBar>
      <Container>
        <BoxSample />
        <MenuSample />
      </Container>
    </ThemeProvider>
  )
}

function BoxSample() {
  return (
    <>
      <Typography variant='h3'>Box example</Typography>
      <Box style={{ display: 'flex', flexDirection: 'row' }}>
        <Box width={1 / 4} style={{ padding: '8px' }}>
          <Button
            variant='contained'
            color='secondary'
            style={{ width: '100%' }}>
            Button 1
          </Button>
        </Box>
        <Box width={1 / 2} style={{ padding: '8px' }}>
          <Button
            variant='contained'
            color='secondary'
            style={{ width: '100%' }}>
            Button 1
          </Button>
        </Box>
        <Box width={1 / 4} style={{ padding: '8px' }}>
          <Button
            variant='contained'
            color='secondary'
            style={{ width: '100%' }}>
            Button 1
          </Button>
        </Box>
      </Box>
    </>
  )
}

function MenuSample() {
  const [anchorEl, setAnchorEl] = useState(null)

  const handleClick = (event) => {
    setAnchorEl(event.currentTarget)
  }
  const handleClose = () => {
    setAnchorEl(null)
  }

  return (
    <Box>
      <Button variant='contained' color='secondary' onClick={handleClick}>
        Open Munu
      </Button>
      <Menu
        id='sample-menu'
        anchorEl={anchorEl}
        open={Boolean(anchorEl)}
        onClose={handleClose}>
        <MenuItem onClick={handleClose}>Item 1</MenuItem>
        <MenuItem onClick={handleClose}>Item 2</MenuItem>
        <MenuItem onClick={handleClose}>Item 3</MenuItem>
      </Menu>
    </Box>
  )
}

export default App

```

![[Pasted image 20210617135952.png]]
