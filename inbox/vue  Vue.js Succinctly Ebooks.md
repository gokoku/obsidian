#js/vue 

---
2021-06-18

# Vue.js Succinctly
https://www.syncfusion.com/succinctly-free-ebooks/vuejs-succinctly

ここで、やってみる。

https://www.syncfusion.com/succinctly-free-ebooks/vuejs-succinctly/setup

yarn では global がうまく行かなかった。

```shell
$ npm install -g @vue/cli
$ asdf reshim
$ vue --version

$ vue create test

yarn を選んだ

$ cd test
$ yarn serve
```

http://localhost:8080

![[Pasted image 20210618094937.png]]

### Creating an app (Vue UI)

vue プロジェクトを GUI で管理するダッシュボードのようだ。

グローバルインストール & スタート

```shell
$ vue ui
```

![[Pasted image 20210618095304.png]]

http://localhost:8000/project/select

タブを Create にして

click + Create a new project here

Vue プロジェクトが出来た。つまり、これが vue create project_name だ。

![[Pasted image 20210618102453.png]]

なんじゃいこりゃー

![[Pasted image 20210618095947.png]]

ダークモードは右下の水滴アイコン。

![[Pasted image 20210618113037.png]]


Customize から Run task をチョイスする。

![[Pasted image 20210618101920.png]]

Widget が追加されたので、

![[Pasted image 20210618102004.png]]

Server が起動できるように設定する。

![[Pasted image 20210618102020.png]]

http://localhost:8080/

普通に Vue が起動した。

つまり、これはプラグインの追加とか、サーバの起動とか、いろいろを CUI でやらなくていいダッシュボードだ。

そして、一つ一つを一覧出来る。

# CHAPTER 2 App Basics

https://www.syncfusion.com/succinctly-free-ebooks/vuejs-succinctly/app-basics

server で起動させる。

Tasks --> serve --> で Open app を clickすると <em>Hot reload</em> でこのプロジェクトの Vue がブラウザで立ち上がる。

#### Vue custmize

![Relationships between Vue Components in the Finished Flutter App](https://www.syncfusion.com/books/vuejs-succinctly/Images/relationships-between-vue-components-in-the-finished-flutter-app.png)

こんなイメージで作るらしい。


###### v-for で引っかかった
Vue2 でやったが、

```
4:3 error Elements in iteration expect to have 'v-bind:key' directives vue/require-v-for-key
```

```vue
  <div v-for-key="item in items">
    <h3>{{item.name}}</h3>
    <p>{{item.exp}}</p>
  </div>
```

これでうまくいった。 が、ここではこうだった。

```vue
<div v-bind:key="item.id" v-for="item in items">
```


# CHAPTER3 Expanding the App: UI

app --> Docs --> Item

```shell
$ cd test

$ vue add vuetify
```

せっかくだから Vue UI でプラグインインストールしたほうがよくね?

Icon が出ない。　mdi を使うようにすることで出るようにした。

```shell
$ yarn add @mdi/font
```

記事が古いらしく、出るようにした今、main.js はこうなっている。

```vue
import Vue from 'vue'
import App from './App.vue'
import vuetify from './plugins/vuetify'
import '@mdi/font/css/materialdesignicons.css'

Vue.config.productionTip = false

new Vue({
  vuetify,
  render: (h) => h(App),
}).$mount('#p')
```

+アイコン  https://materialdesignicons.com/

この一覧のアイコンの名前に mdi- を付けると出るようだ。

```vue

//add icon
	<v-btn big color="pink" dark absolute bottom right fab>
         <v-icon>mdi-plus</v-icon>
       </v-btn>

//delete_forever

    <v-btn flat icon color="red lighten-2">
      <v-icon>mdi-delete</v-icon>
    </v-btn>

//done_outline

<v-icon color\="green lighten-1"\>mdi-check-outline</v-icon\>

//worning
<v-icon color\="red lighten-1"\>mdi-alert</v-icon\>
```


なんか、cli で install したほうがいいようだ。vue ui から入れたらなんか変になった。

```shell
npm install moment --save
```


どうも、vuetify の構造がよくわからん。

https://qiita.com/rubytomato@github/items/784a0eb013a9de1937bd

なんか tag 名が変わってる!! Vuetify.js 2.2 でガラッと変わったかな?

https://vuetifyjs.com/ja/components/lists/[[section-30a230af30b730e730f3306830a230a430c630e030b030eb30fc30d7]]

取り敢えずここまでにしとく。

https://www.syncfusion.com/succinctly-free-ebooks/vuejs-succinctly/expanding-the-app-ui

main.js

```js
import Vue from 'vue'
import App from './App.vue'
import vuetify from './plugins/vuetify'
import '@mdi/font/css/materialdesignicons.css'

Vue.config.productionTip = false

new Vue({
  vuetify,
  render: (h) => h(App),
}).$mount('#p')
```



App.vue

```js
<template>
<div id="app">
  <v-app>
    <Doc ref="modal" :item="newdoc" :items="items" />
    <v-layout row>
      <v-flex xs12 sm6 offset-sm3>
        <v-card>
          <Docs :items="items"/>
          <v-card-text tyle="height: 100px; position: relative">
            <v-btn big color="pink" dark absolute bottom right fab>
              <v-icon>mdi-plus</v-icon>
            </v-btn>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </v-app>
</div>
</template>

<script>
import Docs from './components/Docs'
import Doc from './components/Doc'

export default {
  name: 'App',
  components: {
    Docs, Doc
  },
  methods: {
    openModel() {
      this.$refs.modal.show();
    }
  },

  data: () => ({
    newdoc: {
      id: -1,
      name: "New Document",
      exp: "",
      alert1year: true,
      alert6months: true,
      alert3months: true,
      alert1month: true
    },
    items: [
      {id: 1, name: "Test 1", exp: "16 Oct 2021"},
      {id: 2, name: "Test 1", exp: "16 Nov 2021"}
    ]
  }),
};
</script>

<style lang='scss' scoped>
</style>
```

Document.vue

```js
<template>
<div id="app">
  <v-app>
    <Doc ref="modal" :item="newdoc" :items="items" />
    <v-layout row>
      <v-flex xs12 sm6 offset-sm3>
        <v-card>
          <Docs :items="items"/>
          <v-card-text tyle="height: 100px; position: relative">
            <v-btn big color="pink" dark absolute bottom right fab>
              <v-icon>mdi-plus</v-icon>
            </v-btn>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </v-app>
</div>
</template>

<script>
import Docs from './components/Docs'
import Doc from './components/Doc'

export default {
  name: 'App',
  components: {
    Docs, Doc
  },
  methods: {
    openModel() {
      this.$refs.modal.show();
    }
  },

  data: () => ({
    newdoc: {
      id: -1,
      name: "New Document",
      exp: "",
      alert1year: true,
      alert6months: true,
      alert3months: true,
      alert1month: true
    },
    items: [
      {id: 1, name: "Test 1", exp: "16 Oct 2021"},
      {id: 2, name: "Test 1", exp: "16 Nov 2021"}
    ]
  }),
};
</script>

<style lang='scss' scoped>
</style>
```

Doc.vue

```js
<template>
<div>
  <v-dialog v-model="showModel"
    fullscreen
    hide-overlay
    transition="dialog-bottom-transition"
    scrollable
  >
    <v-card tile>
      <v-toolbar color="blue" dark>
        <v-btn icon dark @click="hide">
          <v-icon>mdi-close</v-icon>
        </v-btn>
        <v-toolbar-title>{{item.name}}</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-items>
          <v-btn dark flat @click="save">Save</v-btn>
        </v-toolbar-items>
      </v-toolbar>
      <v-card-text>
        <v-list three-line subheader>
          <v-list-tile avatar>
            <v-list-tile-content>
              <v-list-tile-title>
                Document Name
              </v-list-tile-title>
              <v-listtile-sub-title></v-listtile-sub-title>
              <v-text-field
                ref="name"
                v-model="name"
                :rules="[() => !name || 'Required field']"
                required
                >
              </v-text-field>
            </v-list-tile-content>
          </v-list-tile>
          <v-list-tile avatar>
            <v-list-tile-content>
              <v-list-tile-title>
                Expiry Date
              </v-list-tile-title>
              <v-list-tile-sub-title></v-list-tile-sub-title>
              <v-menu
                ref="menu"
                v-model="menu"
                :close-on-content-click="false"
                :nudge-right="40"
                :return-value.sync="date"
                lazy
                transition="scale-transition"
                offset-y
                full-width
                min-width="290px"
                >
                <template v-slot:activator="{on}">
                  <v-text-filed
                    v-model="date"
                    prepend-icon="event"
                    readonly
                    v-on="on"
                    ></v-text-filed>
                </template>
                <v-date-picker
                  v-mode="date"
                  :min=date
                  max="2099-12-31"
                  no-title scrollable>
                  <v-spacer></v-spacer>
                  <v-btn flat color="primary"
                    @click="menu=false">Cancel</v-btn>
                  <v-btn flat color="primary"
                    @click="$refs.menu.save(date)">OK</v-btn>
                </v-date-picker>
              </v-menu>
            </v-list-tile-content>
          </v-list-tile>
        </v-list>
        <v-divider></v-divider>
        <v-list four-line subheader>
          <v-list-tile avatar>
            <v-list-tile-action>
              <v-switch
                v-model="alert1year"
                :label="`Alert @ 1.5 & 1 year(s)`"
              ></v-switch>
            </v-list-tile-action>
          </v-list-tile>
          <v-list-tile avatar>
            <v-list-tile-action>
              <v-switch
                v-model="alert6months"
                :label="`Alert @ 6 months`"
              ></v-switch>
            </v-list-tile-action>
          </v-list-tile>
          <v-list-tile avatar>
            <v-list-tile-action>
              <v-switch
                v-model="alert3months"
                :label="`Alert @ 3 months`"
              ></v-switch>
            </v-list-tile-action>
          </v-list-tile>
          <v-list-tile avatar>
            <v-list-tile-action>
              <v-switch
                v-model="alert1months"
                :label="`Alert @ 1 month or less`"
              ></v-switch>
            </v-list-tile-action>
          </v-list-tile>
        </v-list>
      </v-card-text>

      <div style="flex: 1 1 auto;"></div>
    </v-card>
  </v-dialog>
</div>
</template>
<script>
export default {
  name: "Doc",
  props: ['item'],
  data: () => {
    return {
      name: "",
      date: new Date().toISOString.substr(0, 10),
      alert1year: true,
      alert6months: true,
      alert3months: true,
      alert1month: true,
      menu: false,
      showModal: false
    }
  },
  methods: {
    show() {
      this.showModal = true;
    },
    hide() {
      this.showModal = false;
    },
    save() {
      this.showModal = false;
    }
  }
}
</script>
<style lang='scss' scoped>
</style>
```


Item.vue

```js
<template>
<div>
  <Doc ref="modal" :item="item" :items="items" />

  <div>
    <v-list-item-icon>
      <v-btn @click="removeItem" flat icon color="red lighten-2">
        <v-icon>mdi-delete</v-icon>
      </v-btn>
      <v-btn @click="openModal" flat icon color="green lighten-2">
        <v-icon>mdi-pencil</v-icon>
      </v-btn>
    </v-list-item-icon>

    <v-list-content>
      <v-list-title class="text-primary">
        {{ item.name }}
      </v-list-title>
      <v-list-subtitle>
        {{ item.exp }}
      </v-list-subtitle>
      <v-list-subtitle>
        {{ dayLeft(item.exp)}}
      </v-list-subtitle>
    </v-list-content>

    <v-list-item-action>
      <v-list-item-action-text>
        {{ item.id }}
      </v-list-item-action-text>
      <v-icon v-if="!hasExpired(item.exp)"
        color="green lighten-1">mdi-check-outline</v-icon>
      <v-icon v-else color="red lighten-1">mdi-alert</v-icon>
    </v-list-item-action>
  </v-list-item>
  <v-divider v-if="item.id + 1 < lgth"
      :key="`divider-${item.id}`">
  </v-divider>
</div>
</template>
<script>
import Doc from './Doc'
import moment from 'moment'

export default {
  name: "Item",
  components: {
    Doc
  },
  props: ["item", "lgth", "items"],
  methods: {
    openModal() {
      this.$refs.modal.show()
    },
    removeItem() {
      let idx = this.items.indexOf(this.item)
      if(idx > -1) {
        this.items.splice(idx, 1)
      }
    },
    dayLeft(dt) {
      let ends = moment(dt)
      let starts = moment(Date.now())
      let years = ends.diff(starts, "year")
      starts.add(years, "years")
      let months = ends.diff(starts, "months")
      starts.add(months, "months")
      let days = ends.diff(starts, "days")
      let rs = years + "years" + months + "months" + days + "days"
      return this.hasExpired(dt) ?
        "Expired" + rs.replace(/-/g, '') + "ago" : "Left: " + rs
    },
    hasExpired(dt) {
      return (Date.parse(dt) <= Date.now()) ? true : false
    }
  }
}
</script>
<style lang='scss' scoped>
</style>
```

完動はしてないよ。

この続きはデバッグして、同じように動くようにすることから。