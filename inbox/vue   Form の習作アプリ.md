#js/vue 

## 連動セレクト その他+テキストフィールド


src/components/Vorm.vue

```js
<template>
  <div class="w-1/2 p-5 m-auto">
    <div class="w-full">
      <h1 class="text-2xl font-bold">今日の体温</h1>
      <h2 class="mb-2">出社前に測った体温を入力しよう！ <span class="text-red-700 text-sm">※必須</span></h2>
    </div>
    <form action="">
      <div class="mb-6">
        <label class="block text-gray-700 text-sm font-bold mb-2" for="temperature">
          今日の体温
          <span class="text-red-700 text-sm">※</span>
        </label>
        <input
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outlien-none focus:shadow-outline"
          type="number"
          name="temperature"
          id="temperature"
          :value="temperature"
          placeholder="体温を入力してください"
        >
      </div>

      <div class="mb-6">
        <label class="block text-gray-700 text-sm font-bold mb-2" for="cond">
          体調に関して
          <span class="text-red-700 text-sm">※</span>
        </label>
        <p v-for="(condition, i) in conditions" :key="i" >
          <input
            class="w-5 h-5"
            type="checkbox"
            name="cond"
            v-model="cond"
            :value="condition"
          > {{ condition }}
        </p>
        <div v-show="isOther">
          <input
            class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outlien-none focus:shadow-outline"
            type="text"
            name="cond_others"
            placeholder="詳しくご記入ください"
          >
        </div>

      </div>
      <div class="mb-6">
        <label class="block text-gray-700 text-sm font-bold mb-2" for="others">
          その他補足や連絡事項
          <br>
          <span class="text-sm">補足や連絡事項などありましたら記入してください</span>
        </label>
        <input
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outlien-none focus:shadow-outline"
          type="text"
          name="others"
          id="others"
          v-model="others"
          placeholder="ご記入ください"
        >
      </div>

      <div class="mb-6">
        <label class="block text-gray-700 text-sm font-bold mb-2" for="others">
          所属
          <span class="text-red-700 text-sm">※</span>
        </label>
        <select v-model="selectedSection">
          <option v-for="(value, i) in sections" :key="i">{{ value }}</option>
        </select>
      </div>

      <div class="mb-6">
        <label class="block text-gray-700 text-sm font-bold mb-2" for="others">
          登録者
          <span class="text-red-700 text-sm">※</span>
        </label>
        <select v-model="member">
          <option v-for="(value, i) in person" :key="i">{{ value }}</option>
        </select>
      </div>

      <button type="submit">Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Form',
  data() {
    return {
      temperature: Number,
      conditions: [
        "健康です",
        "発熱",
        "37.5度以上の発熱が続いている",
        "咳",
        "痰が絡む",
        "喉が痛い",
        "倦怠感",
        "嗅覚、味覚の障害",
        "その他"
      ],
      cond: [],
      cond_other: "",
      others: "",
      selectedSection: "",
      persons: [],
      member: ""
    }
  },
  created() {
    this.persons = {
      "本社 管理部": ["山崎 浩幸", "渡辺 亜由美", "長野 夕香里", "前田 幸子"],
      "本社 営業部": ["井沢 誠賢", "南雲 康代", "家子 美由紀", "久保 里緒菜", "藤原 琉伊"],
      "本社 SS": ["橋本 誠", "福田 理法", "佐藤 貴規", "須藤 彩斗", "菊地 崇", "山崎 伸浩", "永浜　美月"],
      "本社 SA": ["寺内 義教", "分玉 修央", "川嶋 晃輔", "塚島 里夏", "清水畑 朋子", "浜津 啓章", "飯田 竜太", "小野寺 佑美", "須貝 真由美"],
      "本社 CS": ["後藤 幸司", "會美 知史", "松田 慧介", "平林 浩一", "千田 達寛", "斎藤 翔", "濱田 萌絵", "布施 拓哉", "新宅 和佳子", "松本 歩", "松本 百利"],
      "本社 SD": ["渡邉 一", "豊田 訓之", "阿部 葉月", "山本 剛", "稲垣 貴大", ""],
      "遠野事業所": ["菊池 朋宏", "菊池 亜希", "菊池 里美", "澁谷 一朗"],
      "遠野ICT": ["佐々木 政洋", "林 裕子", "松田 環", "菅田 恵美子", "笹村 美希", "菅原 祐", "新田 真理子", "佐々木 昌子", "佐藤 優美", "小水内 美里"],
      "盛岡 第1 (104)": ["宮城 有作", "多田 穰司", "田中 悟", "佐々木 雪乃", "平野 翔子", "高橋 健太"],
      "盛岡 第2 (2-1)": ["及川 淳", "荻原 弘章", "伊藤 光", "吉田裕範", "安達 優人", "畠山 徹", "阿部 翔", "川村 智子", "及川 茉由佳"]
    }
  },
  computed: {
    sections() {
      return Object.keys(this.persons)
    },
    person() {
      return this.persons[this.selectedSection]
    },
    isOther() {
      console.log("******:" + this.cond.includes('その他'))
      return this.cond.includes('その他')
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>

```

![](form.png)

* * *

## 連動するセレクトを実装する

データを JSON オブジェクトにして、key だけでのセレクトを用意。

```js
  computed: {
    sections() {
      return Object.keys(this.persons)
    },
```

選ばれた値をselectedSection に入れる。

```js
        <select v-model="selectedSection">

```

この selectedSection が変わったら、その子配列が入るようにする。

```js
  computed: {
    person() {
      return this.persons[this.selectedSection]
    },
```

子配列でセレクト要素をつくり、選んだのが member に入るようにする。

```js
        <select v-model="member">
          <option v-for="(value, i) in person" :key="i">{{ value }}</option>
        </select>

```

## その他が選択されたらテキストフィールドが出るようにする

```js
        <p v-for="(condition, i) in conditions" :key="i" >
          <input
            class="w-5 h-5"
            type="checkbox"
            name="cond"
            v-model="cond"
            :value="condition"
          > {{ condition }}
        </p>
        <div v-show="this.cond.includes('その他')">
          <input
            class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outlien-none focus:shadow-outline"
            type="text"
            name="cond_others"
            placeholder="詳しくご記入ください"
          >
        </div>
```


まだ間違ってるかもしれないが、とりあえずバインドでリアクティブな動きをしてることがうっすら染みてきてる状態。

実際のフォームとして POST のイメージがほしい。

Ajax で send して、終了ページ遷移するためにルーターがいるということか。
