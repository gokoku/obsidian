---
type: note
---

	#gtfs #elixir

---
2023-02-15  17:49

# GTFS の調査・佐賀の生データを見る2

livebook で解析をしてるが、地図表示がしたい!!

https://qiita.com/RyoWakabayashi/items/1a12439d2971444ba33b


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/RyoWakabayashi/items/1a12439d2971444ba33b" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9RWxpeGlyJTIwTGl2ZWJvb2slMjAlRTMlODElQTclRTUlOUIlQkQlRTUlOUMlOUYlRTQlQkElQTQlRTklODAlOUElRTclOUMlODElRTMlODElQUUlRTglQTElOEMlRTYlOTQlQkYlRTUlOEMlQkElRTUlOUYlOUYlRTMlODMlODclRTMlODMlQkMlRTMlODIlQkYlRTMlODIlOTIlRTUlOEYlQUYlRTglQTYlOTYlRTUlOEMlOTYlRTMlODElOTklRTMlODIlOEIlRUYlQkMlODglRTklODMlQkQlRTklODElOTMlRTUlQkElOUMlRTclOUMlOEMlRTMlODMlQkIlRTUlQjglODIlRTUlOEMlQkElRTclOTQlQkElRTYlOUQlOTElRTMlODElQUUlRTUlQkQlQTIlRTMlODIlOTIlRTUlOUMlQjAlRTUlOUIlQjMlRTQlQjglOEElRTMlODElQUIlRTMlODMlOTclRTMlODMlQUQlRTMlODMlODMlRTMlODMlODglRTMlODElOTklRTMlODIlOEIlRUYlQkMlODkmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT01NiZ0eHQtY2xpcD1lbGxpcHNpcyZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTY2OGNkZDMxOWFiMGE1MTIxZGZhNDY4YWVhZTEyNzY1&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwUnlvV2FrYWJheWFzaGkmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTBhZTk3NDk3MTBjODE5ZTMxODMyMThhMzc2NzNmNGM2&blend-x=142&blend-y=491&blend-mode=normal&s=66451456d5b8ce2e62946c4fcd6940bf')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Elixir Livebook で国土交通省の行政区域データを可視化する（都道府県・市区町村の形を地図上にプロットする） - Qiita</h1>
		<p class="rich-link-card-description">
		はじめに みなさん、都道府県・市区町村の位置と形、分かりますか？ 我が家では日本地図パズルで娘とよく勉強したものです  都道府県・市区町村の形が分かると何が良いのか、、、 都道府県・市区町村の地理情報=座標=緯度経度があれば、ある地...
		</p>
		<p class="rich-link-href">
		https://qiita.com/RyoWakabayashi/items/1a12439d2971444ba33b
		</p>
	</div>
</a></div>

ここを参考にして佐賀県地図を表示したい。

なんとqiitaの記事にしてしまった!!

<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/hozyo/items/0ea0c253b652fba0f836" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9RWxpeGlyJTIwTGl2ZWJvb2slMjAlRTMlODElQTclRTQlQkQlOTAlRTglQjMlODAlRTclOUMlOEMlRTMlODElQUVHVEZTJUUzJTgxJThCJUUzJTgyJTg5JUUzJTgzJTkwJUUzJTgyJUI5JUU1JTgxJTlDJUUzJTgyJTkyJUU1JTlDJUIwJUU1JTlCJUIzJUUzJTgxJUFCJUU4JTkwJUJEJUUzJTgxJUE4JUUzJTgxJTlCJUUzJTgxJTlGJUUzJTgyJTg5JUUzJTgxJTg0JUUzJTgxJTg0JUUzJTgxJUFBJUUzJTgzJUJDJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz04ZmU1OGI3Y2EwYTBlNDEwMGY3OTY3ZmY3OWFiMjcyMQ&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwaG96eW8mdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTA3MjQwNTM5ODcxMDY3NjM4M2MzMWIxODYyNmJhN2E4&blend-x=142&blend-y=491&blend-mode=normal&s=72c23031df7083f627a1083c46c12d87')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Elixir Livebook で佐賀県のGTFSからバス停を地図に落とせたらいいなー - Qiita</h1>
		<p class="rich-link-card-description">
		はじめに 「GTFSを使うことになったから調べてて。」とのこと。 何のことでしょう。GTFSって何? 路線バス情報? データですか? ならば「@nako_sleep_9hさん主催のpiyopiyo.ex #14：Elixirでデータ...
		</p>
		<p class="rich-link-href">
		https://qiita.com/hozyo/items/0ea0c253b652fba0f836
		</p>
	</div>
</a></div>

ドキュメントをまとめられて、尚且つみんなに共有できてElixir界隈にちっちゃく力添えもできて、みんないい。