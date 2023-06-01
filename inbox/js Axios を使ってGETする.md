#js

---
2022-04-18  10:53

# node Axios を使ってGETする


<div class="rich-link-card-container"><a class="rich-link-card" href="https://github.com/axios/axios" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://opengraph.githubassets.com/6a0565753bd2fe7b6936173e8a7606ae970ce4140044b8f0d5b7bb5ce21580b8/axios/axios')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">GitHub - axios/axios: Promise based HTTP client for the browser and node.js</h1>
		<p class="rich-link-card-description">
		Promise based HTTP client for the browser and node.js - GitHub - axios/axios: Promise based HTTP client for the browser and node.js
		</p>
		<p class="rich-link-href">
		https://github.com/axios/axios
		</p>
	</div>
</a></div>

```shell
$ npm i axios
```

```ts
import axios, { AxiosError, AxiosResponse } from 'axios'

axios({
  method: 'get',
  url: 'http://localhost:3001/templates',
  responseType: 'json'
}).then((response: AxiosResponse) => {
  console.log(response.data)
  const template = response.data
  dc.build(template)
  dc.draw() //プレビュー用

}).catch((error: AxiosError) => {
  console.log(error)
})
```

