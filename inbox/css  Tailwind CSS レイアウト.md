#css/tailwind 


## 1カラム系

App.vue

```js
<template>
  <div class="container mx-auto px-20">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>
```

components/HelloWorld.vue

```js
<template>
  <div class="md:flex border rounded-lg m-10 px-3 py-4">
    <img class="mk:flex-shrink-0 bg-gray-300 rounded-lg px-4 py-2 w-40 h-30" alt="Vue logo" src="../assets/logo.png">
    <div class="mt-4 md:mt-0 md:ml-6">
      <h1 class="text-lg mt-1 leading-tight font-semibold ">{{ msg }}</h1>
      <button class="mt-2 bg-indigo-700 btn">button</button>
    </div>
  </div>
</template>
```

