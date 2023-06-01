#js/vue 


[https://blog.proglearn.com/2020/03/31/【nuxt-js-x-moment-js】日時表示操作を簡単にしよう！-nuxt-jsにmoment-js/](https://blog.proglearn.com/2020/03/31/%E3%80%90nuxt-js-x-moment-js%E3%80%91%E6%97%A5%E6%99%82%E8%A1%A8%E7%A4%BA%E6%93%8D%E4%BD%9C%E3%82%92%E7%B0%A1%E5%8D%98%E3%81%AB%E3%81%97%E3%82%88%E3%81%86%EF%BC%81-nuxt-js%E3%81%ABmoment-js/)

```bash
$ yarn add --dev @nuxtjs/moment
```

nuxt.config.js

```bash
export default {
	buildModules: [
		'@nuxtjs/moment'
	],
	moment: {
		// ここにオプションが記述できる
		locales: ['ja']
	}
}
```

使い方

```bash
$moment().format('YYYY-MM-DD')
```

これが通じないところでは store の中とかは

```bash
import moment from 'moment'

moment().format('YYYY-MM-DD')
```

