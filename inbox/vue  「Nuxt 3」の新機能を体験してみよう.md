#js/vue 

---
2022-02-22  19:11

# vue  「Nuxt 3」の新機能を体験してみよう


<div class="rich-link-card-container"><a class="rich-link-card" href="https://codezine.jp/article/detail/15492" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://codezine.jp/static/images/article/15492/1200.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">「Nuxt 3」の新機能を体験してみよう――環境作成からVue 3の新機能活用・Nuxt Bridgeの利用方法まで</h1>
		<p class="rich-link-card-description">
		本連載では、Webページのユーザーインタフェース（UI）構築に「Vue 3」を利用したフレームワーク「Nuxt 3」の活用方法を紹介します。初回となる今回は、Nuxt 3の内容と現状を説明するとともに、Nuxt 3のプロジェクトを生成して動作を確認します。また、前バージョン「Nuxt 2」のプロジェクトにNuxt 3の機能を導入する「Nuxt Bridge」の利用法を説明します。
		</p>
		<p class="rich-link-href">
		https://codezine.jp/article/detail/15492
		</p>
	</div>
</a></div>

VScode extention

![[Pasted image 20220222191823.png]]


Nuxt3 の CLIツールで生成。

```shell
$ npx nuxi init p001-next3-plain

$ cd p001-next3-plain
$ npm i
$ npm run dev
```

http://localhost:3000

![[Pasted image 20220222194011.png]]

```vue
<template>
  <div>
    <div>
      <input v-model="family"><input v-model="given">
    </div>
    <div>名前: {{ fullName }}</div>
    <button @click="onClickGreet">あいさつ</button>
  </div>
</template>
<script lang="ts">
export default defineComponent({
  setup() {
    const family = ref('吉川')
    const given = ref('英一')
    const fullName = computed(() => `${family.value} ${given.value}`)
    const onClickGreet = () => alert(`こんにちは、${fullName.value}さん`)
    return {
      family,
      given,
      fullName,
      onClickGreet
    }

  }
})
</script>
```

![[Pasted image 20220222201916.png]]

defineComponent setup ref computed の  import は Nuxt 3 が自動でやってくれてるとのこと。

.nuxt/auto-import.d.ts にある

### nuxt 2

Nuxt Bridge を使って色々めんどくさいことをする必要があるようだ。
過渡期か。

