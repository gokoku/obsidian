#js/react 

---
2021-09-03

# useEffect を使った非同期リクエストサンプル


```shell
$ npx init vite@latest github-user
$ cd github-user
$ npm i
$ npm run dev
```

App.jsx
```jsx
import React, { useState, useEffect } from 'react'
import './App.css'

function GitHubUser({login}) {
  const [data, setData] = useState()
  const [error, setError] = useState()
  const [loading, setLoading] = useState(false)

  useEffect(() => {
    if(!login) return
    setLoading(true)
    fetch(`https://api.github.com/users/${login}`)
    .then(response => response.json())
    .then(setData)
    .then(() => setLoading(false))
    .catch(setError)
  },[login])

  if(error)
    return <pre>{JSON.stringify(error, null, 2)}</pre>
  if (loading) return <h1>loading...</h1>
  if(!data) return null

  return (
    <div className="githubUser">
      <img src={data.avatar_url} alt={data.login} style={{width: 200}}/>
      <div>
        <h1>{data.login}</h1>
        <pre>{JSON.stringify(data, null, 2)}</pre>
      </div>
    </div>
  )
}
function App() {
  return (
    <div className="App">
      <GitHubUser login="gokoku" />
    </div>
  )
}

export default App
```

非同期リクエストには 3つの状態がある。
* 成功 ( fulfilled )
* 失敗 ( rejected )
* 保留中

それぞれを正しくハンドリングすることとのこと。

![[Pasted image 20210903115933.png]]