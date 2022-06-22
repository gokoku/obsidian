#js/p5 #js/vue 

---
2021-07-01

# vue3 で router

### Vue 3 での Router 有効化

src/main.js

```js
import router from '@/router'

createApp(App).use(router)
```

### $router を使って遷移出来なかった

Home view ページから click で別ページに飛ばしたかったが、うまくいかなかった。

src/views/Home.vue

```js
<template>
  <div>
    <h1>ホーム</h1>
    <p><button @click="profile">プロフィール</button></p>
  </div>
</template>
<script>
import { defineComponent } from '@vue/runtime-core'

export default defineComponent({
  setup(props, context) {

    const profile = () => {
      context.root.$router.push(
        'profile',
        () => {},
        () => {},
      )
    }
```

context の中に root が無い模様。

一応 router/index.js でバスは作られてるので、URL 叩けば行く。

### useRouter を使うとうまくいく

```js
<script>
import { defineComponent } from '@vue/runtime-core'

import { useRouter } from 'vue-router'

export default defineComponent({
  setup(props, context) {
    
    const router = useRouter()

    const profile = () => {
      router.push(
        'profile',
        () => {},
        () => {},
      )
    }
```

うまく飛ぶ。