#js/vue 


「たった1日で基本が身につくVue.js」

p130 色を変化させるアプリを作ろう


```shell
$ vue create color
$ cd color
$ yarn serve&
```

src/App.vue

```js
<template>
  <div id="app">
    <div class="slider">
    <p>
      <span>Red:{{ red }}</span>
      <input v-model="red" type="range" max="255" min="0"/>
    </p>
    <p>
      <span>Green:{{ green }}</span>
      <input v-model="green" type="range" max="255" min="0"/>
    </p>
    <p>
      <span>Blue:{{ blue }}</span>
      <input v-model="blue" type="range" max="255" min="0"/>
    </p>
    </div>

    <div class="box" :style="bindStyle"></div>
  </div>
</template>チュートリアル
```

```js
<script>
export default {
  name: 'App',
  data: function() {
    return {
      range: 150,
      red: 0,
      blue: 0,
      green: 0
    }
  },
  computed: {
    bindStyle() {
      return `width: ${this.range}px; height: ${this.range}px; background: rgb(${this.red},${this.green},${this.blue})`
    }
  }
}
</script>
```

```css
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: [[2c3e50]];
  margin-top: 10px;
  display: flex;
  flex-direction: row;
}
.slider {
  display: block;
  text-align: left;
  flex-direction: column;
}
.slider p {
  margin: 5px;
}
.slider p span {
  font-weight: bold;
  display: block;
}
.slider input[type="range"] {
  height: 10px;
}

.box {
  display: flex;
}
</style>

```
