#js/vue 


## JavaScriptフレームワーク「Vue.js」と「Nuxt.js」の活用からチョイス

ルーチング

[https://codezine.jp/article/detail/11828](https://codezine.jp/article/detail/11828)

_name というディレクトリで下位のURLの [params](http://params.name) から取れるくだりの迫力

Vuexのチュートリアル

[https://codezine.jp/article/detail/11994](https://codezine.jp/article/detail/11994)

![](image-kn9lrdvh.png)

非同期でストアのアクションが動くくだりの迫力

非同期のチュートリアル

[https://codezine.jp/article/detail/12398](https://codezine.jp/article/detail/12398)

とってきたJSONがそのままディレクティブで取得出来てしまうくだりの迫力

error catch を async ではどうやるの？

```bash
async asyncData({ $axios, params, error }) {
	try {
		const await res = $axios.get(`http://localhost:3001/station/${params.id}`)
		return res.data
	} catch(e) {
		error({ statusCode: 404, message: 'ページが見つかりません' })
	}
}
```

