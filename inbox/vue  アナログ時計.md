#js/vue

---
2021-09-30

# アナログ時計

https://ics.media/entry/210929/

この中で気に入ったところ。

![[Pasted image 20210930102417.png]]

```shell
$ npm init vite@latest fat-clock
$ cd fat-clock
$ npm i sass --save-dev
$ npm run dev
```

App.vue

```js:App.vue
<template>
<div class="FatClock">
  <div class="clock">
    <div class="hand hour" :style="{transform: `rotate(${angles.hour}deg`}"></div>
    <div class="hand minute" :style="{transform: `rotate(${angles.minute}deg`}"></div>
    <div class="hand second" :style="{transform: `rotate(${angles.second}deg`}"></div>
    <div class="timeText">{{ timeText }} </div>
  </div>
</div>


</template>

<script>
export default{
  data() {
    return {
      current: new Date(),
      timer: 0
    }
  },
  computed: {
    timeText() {
      const d = this.current
      const [h, m, s] = [d.getHours(), d.getMinutes(), d.getSeconds()]
      return [h, m, s].map((num) => String(num).padStart(2, '0')).join(":")
    },
    angles() {
      const d = this.current
      const [h, m, s] = [d.getHours(), d.getMinutes(), d.getSeconds()]
      return {
        hour: (((h % 12) + m / 60) /12) * 360 - 90,
        minute: ((m + s / 60) / 60) * 360 - 90,
        second: (s / 60) * 360 - 90
      }
    }
  },
  methods: {
    updateTime() {
      this.current = new Date()
    }
  },
  mounted() {
    this.timer = window.setInterval(() => this.updateTime(), 100)
  },
  unmounted() {
    window.clearInterval(this.timer)
  }

}
</script>

<style lang="scss" scoped>
$clock-size: 300px;
.FatClock {
  position: relative;
  width: 300px;
  height: 300px;
  border: 2px solid currentColor;
  border-radius: 50%;
}
.clock {
  position: absolute;
  width: 0px;
  height: 0px;
  left: 50%;
  top: 50%;

  .hand {
    position: absolute;
    transform-origin: 6px 1px;
    height: 2px;
    top: -1px;
    left: -6px;
    background-color: currentColor;
    &.hour {
      width: $clock-size * 0.25;
    }
    &.minute {
      width: $clock-size * 0.4;
    }
    &.second {
      width: $clock-size * 0.5;
    }
  }
  .timeText {
    position: absolute;
    width: $clock-size;
    left: $clock-size * -0.5;
    top: 1em;
    font-size: 14px;
    font-weight: bold;
    text-align: center;;
  }
}
</style>
```

これが悪い例とのこと。良くしてみる?

Composition API で関心事を分離するとのこと。

## composition を使ってみた

とりあえず分離してゴニョゴニョして落ち着いたコード。

useClock.js

```js:useClock.js
export const useClock = () => {
  const d = new Date()
  const dTime = {
    hour: d.getHours(),
    minute: d.getMinutes(),
    second: d.getSeconds(),
  }
  const format = (num) => String(num).padStart(2, '0')
  const text = `${format(dTime.hour)}:${format(dTime.minute)}:${format(
    dTime.second,
  )}`
  const aTime = {
    hour: (((dTime.hour % 12) + dTime.minute / 60) / 12) * 360 - 90,
    minute: ((dTime.minute + dTime.second / 60) / 60) * 360 - 90,
    second: (dTime.second / 60.0) * 360 - 90,
  }
  return { dTime, aTime, text }
}
```

App.vue

```js:App.vue
<template>
<div class="FatClock">
  <div class="clock">
    <div class="hand hour" :style="{transform: `rotate(${state.clock.deg.hour}deg`}"></div>
    <div class="hand minute" :style="{transform: `rotate(${state.clock.deg.minute}deg`}"></div>
    <div class="hand second" :style="{transform: `rotate(${state.clock.deg.second}deg`}"></div>
    <div class="timeText">{{ state.clock.text }} </div>
  </div>
</div>
</template>

<script>
import { defineComponent, reactive, watchEffect, onMounted, onUnmounted } from 'vue'
import { useClock } from './useClock'

export default defineComponent({
  setup() {
    let timer = 0
    const state = reactive({
      clock: {
        text: '',
        deg: {hour:-90, minute:-90, second:-90}
      }
    })

    const current = () => {
      const {text, aTime} = useClock()
      state.clock = {...state.clock, text: text, deg: aTime}
    }

    watchEffect(() => console.log(state.clock.deg))
    onMounted(() => timer = window.setInterval(() => current(), 1000))
    onUnmounted(() => window.clearInterval(timer))
    return { state }
  },
})

</script>

<style lang="scss" scoped>
$clock-size: 300px;
.FatClock {
  position: relative;
  width: 300px;
  height: 300px;
  border: 2px solid currentColor;
  border-radius: 50%;
}
.clock {
  position: absolute;
  width: 0px;
  height: 0px;
  left: 50%;
  top: 50%;

  .hand {
    position: absolute;
    transform-origin: 6px 1px;
    height: 2px;
    top: -1px;
    left: -6px;
    background-color: currentColor;
    &.hour {
      width: $clock-size * 0.25;
    }
    &.minute {
      width: $clock-size * 0.4;
    }
    &.second {
      width: $clock-size * 0.5;
    }
  }
  .timeText {
    position: absolute;
    width: $clock-size;
    left: $clock-size * -0.5;
    top: 1em;
    font-size: 14px;
    font-weight: bold;
    text-align: center;;
  }
}
</style>
```