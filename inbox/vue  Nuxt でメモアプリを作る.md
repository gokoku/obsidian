#js/vue 



```shell
$ npx create-nuxt-app my-memo
$ cd my-memo
$ code . && yarn dev
```

tailwindcss を選んだ。

```shell
$ yarn add moment
$ yarn add vuex-persistedstate
```

tailwindcss を選んだけど、この設定がないとライブリロードされない
あってもライブリロードしてくれない。

postcss.config.js

```js
const tailwindcss = require('tailwindcss')
const autoprefixer = require('autoprefixer')

module.exports = {
  plugins: [
    tailwindcss,
    autoprefixer,
  ]
}
```

ここも,@import になってると警告が出るので直す。
assets/css/tailwind.css

```css
@tailwind base;
@tailwind components;

.btn-blue {
  @apply bg-blue-500 text-white font-semibold py-2 px-4 rounded-lg;
}
.btn-blue:hover {
  @apply bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg;
}
.btn-red {
  @apply bg-red-500 text-white font-semibold py-2 px-4 rounded-lg;
}
.btn-blue:hover {
  @apply bg-red-700 text-white font-semibold py-2 px-4 rounded-lg;
}
.input-text {
  @apply bg-white shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight;
}
.input-text:focus {
  @apply outline-none shadow-outline;
}

@tailwind utilities;

```

tailwind.config.js

```js
module.exports = {
  theme: {},
  variants: {},
  plugins: [],
  purge: {
    // Learn more on https://tailwindcss.com/docs/controlling-file-size/[[removing-unused-css]]
    enabled: process.env.NODE_ENV === 'production',
    content: [
      'components/**/*.vue',
      'layouts/**/*.vue',
      'pages/**/*.vue',
      'plugins/**/*.js',
      'nuxt.config.js'
    ]
  }
}
```

* * *

## Store

store/index.js

```js
import createPersistedState from "vuex-persistedstate";

export const plugins = [createPersistedState()];
```

store/memo.js

```js
import moment from "moment";

export const state = () => ({
  memo: [],
  page: 0
});

export const mutations = {
  insert(state, obj) {
    if (obj.title === "" || obj.content === "") return;
    var date = moment().format("YYYY-MM-DD hh:mm");
    state.memo.unshift({
      title: obj.title,
      content: obj.content,
      created: date
    });
  },
  set_page(state, p) {
    state.page = p;
  },
  remove(state, obj) {
    var num = 0;
    state.memo.forEach((memo, i) => {
      if (
        memo.title == obj.title &&
        memo.content == obj.content &&
        memo.created == obj.created
      ) {
        state.memo.splice(i, 1);
        return;
      }
    });
  }
};
```

## Pages

pages/index.vue

```js
<template>
  <div class="flex flex-col items-left max-w-1/2 m-10 px-3 py-4">
    <h1 class="text-xl font-bold">Memo</h1>
    <div class="border round-lg px-2 py-4">
      <div class="mb-3">
        <h2 class="text-base font-bold mr-3">Title</h2>
        <input
          class="input-text w-3/4 mr-3 mb-3"
          type="text"
          name="title"
          v-model="title"
          @focus="set_flg"
        >
        <button
          class="btn-blue"
          @click="find"
        >find</button>
      </div>

      <div class="mb-3">
        <h2 class="text-base font-bold mr-3">Memo</h2>
        <textarea
          class="input-text w-full mr-3"
          name="memo"
          v-model="content"
          @focus="set_flg"
        />
      </div>

      <div class="mb-3">
        <button
          class="btn-blue"
          @click="insert"
        >save</button>
        <button
          v-if="sel_flg != false"
          class="btn-red"
          @click="remove"
        >delete</button>
      </div>
    </div>

    <div class="border round-lg px-2 py-3 mt-4">
      <div class="w-full py-3 text-center">
        <span v-show="!prev_page" class="text-white bg-green-200 rounded px-2 py-2">◀</span> &nbsp;
        <span v-show="prev_page" class="text-white bg-green-500 rounded px-2 py-2" @click="prev">◀</span> &nbsp;

        <span v-show="!next_page" class="text-white bg-green-200 rounded px-2 py-2">▶</span>
        <span v-show="next_page" class="text-white bg-green-500 rounded px-2 py-2" @click="next">▶</span>
      </div>
      <ul class="list">
        <li v-for="(item, index) in page_items" :key="index">
          <span @click="select(item)">{{ item.title }} ({{ item.created }})</span>
        </li>
      </ul>
    </div>

  </div>
</template>

<script>
export default {
  data: function(){
    return {
      title: '',
      content: '',
      num_per_page: 3,
      find_flg: false,
      sel_flg: false
    }
  },
  computed: {
    memo: function() {
      return this.$store.state.memo.memo
    },
    page_items: function() {
      if( this.find_flg ) {
        var arr = []
        var data = this.$store.state.memo.memo
        data.forEach( element => {
          if( element.title.toLowerCase().indexOf(this.title.toLowerCase()) >= 0) {
            arr.push(element)
          }
        })
        return arr
      } else if( this.sel_flg != false ) {
        return [this.sel_flg]
      } else {
        return this.$store.state.memo.memo.slice(
          this.num_per_page * this.$store.state.memo.page,
          this.num_per_page * (this.$store.state.memo.page + 1)
        )
      }
    },
    prev_page: function() {
      return this.page !== 0
    },
    next_page: function() {
      return this.page < this.page_number
    },
    page_number: function() {
      return Math.floor((this.$store.state.memo.memo.length - 1) / this.num_per_page)
    },
    page: {
      get: function() {
        return this.$store.state.memo.page
      },
      set: function(p) {
        var pg = p > this.page_number ? this.page_number : p
        pg = pg < 0 ? 0 : pg
        this.$store.commit('memo/set_page', pg)
      }
    }
  },
  methods: {
    set_flg: function() {
      if( this.find_flg || this.sel_flg != false ) {
        this.find_flg = false
        this.sel_flg = false
        this.title = ''
        this.content = ''
      }
    },
    insert: function() {
      this.$store.commit('memo/insert', {title: this.title, content: this.content})
      this.title = ''
      this.content = ''
    },
    select: function(item) {
      this.find_flg = false
      this.sel_flg = item
      this.title = item.title
      this.content = item.content
    },
    remove: function() {
      if( this.sel_flg == false ) {
        return
      } else {
        this.$store.commit('memo/remove', this.sel_flg)
        this.set_flg()
      }
    },
    find: function() {
      this.sel_flg = false
      this.find_flg = true
    },
    next: function() {
      this.page++
    },
    prev: function() {
      this.page--
    }
  },

  created: function() {
    this.$store.commit('memo/set_page', 0)
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/

</style>

```

# Github に上げた

<https://github.com/gokoku/my-memo>

* * *

静的サイトでジェネレートしたい。

nuxt.config.js

```js
target: "static"
```

```shell
$ yarn add @nuxtjs/tailwindcss   # これがないとエラー

$ yarn generate
```

Vue packages version mismatch でエラー。

-   vue@2.6.12 
-   vue-template-compiler@2.6.11 

この不整合らしい。
これは global が使われて故のエラーだった。

@vue/cli と nuxt を消して、改めて nuxt だけ入れた。

```shell
$ yarn global remove nuxt @vue/cli
$ yarn add nuxt

$ yarn generate
```

global にしないことで、うまくいった。
dist 以下に生成されている。

ここで serve を起動すると、しっかり動く。

これを Github Page に乗せたい。

### test 動かす

まんまでは、test が動かなかった。
@babel/preset-env が見つからないといわれる。
ファイル名が変わっただけのことらしいが、めんどうなので、

```shell
$ yarn add --dev @babel/preset-env

$ yarn test
```

動いた。
