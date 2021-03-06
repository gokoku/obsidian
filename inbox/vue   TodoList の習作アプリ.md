#js/vue 


# 準備


```shell
$ vue create my-todo
$ cd my-todo
$ yarn add tailwindcss
$ yarn add sass-loader node-sass
```

postcss.config.js

```js
const tailwindcss = require('tailwindcss')
const autoprefixer = require('autoprefixer')

module.exports = {
  plugins: [tailwindcss, autoprefixer],
}
```

src/assets/tailwind.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

src/main.js

```js
import '@/assets/tailwind.css'
```

# src/App.vue

```js
<template>
  <div id="app" class="w-1/2 mx-auto px-4 py-5">
    <h1 class="text-3xl font-bold text-gray-500 mb-4">
      Todo list
    </h1>
    <hr>
    <TodoList />
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'

export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>

<style>
body {
  background: #a;
}
</style>
```

# src/components/TodoList.vue

```js
<template>
  <div class="mt-2">
    <p class="mb-4">
      <button
        class="inline px-4 py-2 bg-red-400 rounded-lg font-bold text-white mr-3 hover:bg-red-700"
        @click="purge">Purge</button>
      <span
        class="text-sm text-gray-500"
      >終了してないタスク {{remaining.length}} 個 / 全タスク {{ todos.length }} 個
      </span>

    </p>
    <ul class="flex flex-col list-none mb-4">
      <li class="flex items-center bg-white px-2 py-2 mb-2 rounded-lg shadow-lg" v-for="(todo,index) in todos" :key="index">
        <input
          class="w-5 h-5 mr-2"
          type="checkbox" v-model="todo.isDone">
        <span
          class="text-lg mr-2"
          :class="{ 'text-blue-500 underline': todo.isDone }">{{ todo.title }}</span>
        <span
          class="text-red-500 w-5 h-5 fill-current"
          @click="deleteItem(index)">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M12 0c-6.627 0-12 5.373-12 12s5.373 12 12 12 12-5.373 12-12-5.373-12-12-12zm5.5 16.084l-1.403 1.416-4.09-4.096-4.102 4.096-1.405-1.405 4.093-4.092-4.093-4.098 1.405-1.405 4.088 4.089 4.091-4.089 1.416 1.403-4.092 4.087 4.092 4.094z"/></svg>
          </span>
      </li>
      <li v-show="!todos.length">Todoないよ、やったね</li>
    </ul>
    <form action="" @submit.prevent="addItem">
      <input
        class="shadow-lg appearance-none border rounded-lg w-5/6 py-2 px-3 text-gray-700 mr-1"
        type="text"
        placeholder="タスクを入れてください"
        v-model="newItem">
      <input
        class="px-6 py-2 bg-teal-400 rounded-lg font-bold text-white hover:bg-teal-700"
        type="submit" value="追加">
    </form>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data: function() {
    return {
      newItem: '',
      todos:[]
    }
  },
  watch: {
    todos: {
      handler: function() {
        localStorage.setItem('todos', JSON.stringify(this.todos))
      },
      deep: true
    }
  },
  mounted: function() {
    this.todos = JSON.parse(localStorage.getItem('todos')) || []
  },
  methods: {
    addItem: function() {
      const item = {
        title: this.newItem,
        isDone: false
      }
      if( this.newItem !== '' ) {
        this.todos.push(item)
        this.newItem = ''
      }
    },
    deleteItem: function(index) {
      if(confirm('削除しますか?')) {
        this.todos.splice(index, 1)
      }
    },
    purge: function() {
      if( confirm('終了タスクを全て削除しますか?')) {
        this.todos = this.remaining
      }
    }
  },
  computed: {
    remaining: function() {
      return this.todos.filter((todo) => {
        return !todo.isDone
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
</style>

```

# tailwindcss を自分用に最適化したい

input text のクラスコンポーネント

### src/assets/tailwind.css

ボタン、リスト、テキストフィールドをコンポーネントにしたので、これを使いまわそう。

```css
@tailwind base;
@tailwind components;

.btn {
  @apply inline-block px-5 py-2 rounded-lg font-semibold text-sm tracking-wider;
}
.btn:focus {
  @apply outline-none shadow-outline;
}
.btn-red {
  @apply bg-red-500 text-white;
}
.btn-red:hover {
  @apply transition duration-300 ease-in-out bg-red-600;
}
.btn-red:active {
  @apply bg-red-700;
}
.btn-teal {
  @apply bg-teal-400 text-white;
}
.btn-teal:hover {
  @apply transition duration-300 ease-in-out bg-teal-500;
}
.btn-teal:active {
  @apply bg-teal-600;
}

.input-text {
  @apply bg-white shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight;
}
.input-text:focus {
  @apply outline-none shadow-outline;
}

.col-list {
  @apply flex flex-col list-none;
}
.list-item {
  @apply flex items-center bg-white px-2 py-2 mb-2 rounded-lg shadow;
}

@tailwind utilities;
@tailwind screens;
```

ここでの定義は、疑似クラスは @apply の中にいれられないようだ。
そのかわり、ここで擬似クラスを設定すると、html のクラスでは、擬似クラスを設定しなくてもみんなつくようだ。

```html
<input class="input-text w-full mr-3" ...>
```

これでフォーカスが当たるとホワッと青いシャドウが入る。

