#js/vue 



バーにセレクターを追加する。
しかもコンポーネントとして追加してみた。

compontns/SelectPlaces.vue

```bash
<template>
        <v-select
          @change="handleSelect"
          v-model="select"
          :hint="`${select.name}, ${select.code}`"
          :items="items"
          item-text="name"
          item-value="code"
          label="場所選択"
          presistent-hint
          return-object
          single-line
        >
        </v-select>
</template>
<script>
export default {
  name: "select-places",

  data() {
    return {
      fixed: true,
      title: "Open Weather",
      select: {name: '滝沢市', code: 'Takizawa'},
      items: [
        { name: '札幌', code: 'Sapporo' },
        { name: '滝沢市', code: 'Takizawa' },
        { name: '東京', code: 'Tokyo' },
        { name: '大阪', code: 'Osaka' },
        { name: '高知市', code: 'Kochi' },
        { name: '佐賀市', code: 'Saga' },
        { name: '那覇市', code: 'Naha' },
        { name: '高知市', code: 'Kochi' },
        { name: 'Los Angeles', code: 'Los Angeles' },
        { name: 'New York', code: 'New York' },
        { name: 'London', code: 'London' },
      ],
    }
  },

  methods: {
    async handleSelect() {
      let url = `https://api.openweathermap.org/data/2.5/forecast?q=${this.select.code}&APPID=${process.env.API_KEY}`
      await this.$axios.get(url)
            .then(res => {
              this.$store.commit('initForecasts', res.data)
            })
    }
  }

}
</script>
```

Vuetify のセレクターを使った。
store は this.$store で掴める。
axios も this.$axios で掴める。—>nuxt.config.js で指定済み

store の変更は commit で action を指定する。

これをLayoutのバーに入れる。

layouts/default.vue

```bash
<template>
  <v-app dark>
    <v-app-bar>
    </v-app-bar>
    <v-main>
      <v-container>

        <p>{{ city }}</p>

        <nuxt />

      </v-container>
    </v-main>

  </v-app>
</template>

<script>
import SelectPlaces from '../components/SelectPlaces'

export default {
  name: "layouts",
  components: {
    SelectPlaces
  },

  computed: {
    city() {
      return this.$store.state.city
    }
  },
}
</script>
```

store の掴み方は this.$store
component の使い方。

computed で store の値を出しておくと同期が便利だった。

pages/index.vue

```bash
<template>
  <v-row>
    <v-col v-for="(forecast, index) in $store.state.forecasts" cols="12" sm="3" :key="index">
        <v-card>
          <v-img
            height="100"
            width="100"
            :src="$store.getters.getImgURL(index)"
            >
          </v-img>
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title class="headline">
                {{ $store.getters.getDate(index) }}
              </v-list-item-title>
              <div>
                <span class="large">{{ $store.getters.getHour(index)}}</span>
                <span class="mx-2">{{ $store.getters.getDescription(index) }}</span>
                <span class="large">{{ $store.getters.getTemp(index) }}℃</span>
              </div>
            </v-list-item-content>
          </v-list-item>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  fetch({ $axios, store }) {
    return $axios.get(`https://api.openweathermap.org/data/2.5/forecast?q=Takizawa&APPID=${process.env.API_KEY}`)
    .then(res => {
      store.commit('initForecasts', res.data)
    })
  },
}
</script>
<style>
span.large {
  font-size: 1.2rem;
  font-weight: bold;
}
</style>
```

fetch はマウント時のような感じで使うようだ。

store/index.js

```bash
import moment from 'moment'

export const state = () => ({
  city: '',
  forecasts: new Array(),
})

export const mutations = {
  initForecasts(state, data) {
    state.forecasts = new Array()
    data.list.forEach((elem) => state.forecasts.push(elem))
    state.city = data.city.name
  },
}

export const getters = {
  getImgURL(state) {
    return function (index) {
      const icon = state.forecasts[index].weather[0].icon
      return `http://openweathermap.org/img/wn/${icon}@2x.png`
    }
  },
  getDate(state) {
    return function (index) {
      const date = state.forecasts[index].dt_txt
      return moment(date).format('YYYY年MM月DD日')
    }
  },
  getHour(state) {
    return function (index) {
      const date = state.forecasts[index].dt_txt
      return moment(date).format('HH時')
    }
  },
  getTemp(state) {
    return function (index) {
      const temp = Number(state.forecasts[index].main.temp) - 273.15
      return Math.round(temp * 10) / 10
    }
  },
  getDescription(state) {
    return function (index) {
      return state.forecasts[index].weather[0].description
    }
  },
}
```

![](image-kn9k3lsr.png)
