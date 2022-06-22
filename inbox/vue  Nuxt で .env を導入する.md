#js/vue 

```bash
$ yarn add --dev @nuxtjs/dotenv
```

nuxt.config.js

```bash
import colors from 'vuetify/es5/util/colors'

import dotenv from '@nuxtjs/dotenv'
const { API_KEY } = process.env

export default {

...

build: {},

  env: {
    API_KEY,
  },
}
```

.env

```bash
API_KEY = 'xxxxxxxxxxxxxxxxx'
```

使い方

index.vue

```bash
<script>
export default {
  fetch({ $axios, store }) {
    return $axios.get(`https://api.hoge?api=${process.env.API_KEY}`)
    .then(res => {
      store.commit('initForecasts', res.data.list)
```

process.env.API_KEY で取れる。

