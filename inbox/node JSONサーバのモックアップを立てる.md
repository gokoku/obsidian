#node #js

---
2022-04-18  09:53

# node JSONサーバのモックアップを立てる


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/futoase/items/2859a60c8b240da70572" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9JUUzJTgwJTkwJUU1JTgwJThCJUU0JUJBJUJBJUUzJTgzJUExJUUzJTgzJUEyJUUzJTgwJTkxSlNPTiUyMFNlcnZlciVFMyU4MiU5MiVFNCVCRCVCRiVFMyU4MSVBMyVFMyU4MSVBNiVFNiU4OSU4QiVFMyU4MSVBMyVFNSU4RiU5NiVFMyU4MiU4QSVFNiU5NyVBOSVFMyU4MSU4RldlYkFQSSVFMyU4MSVBRSVFMyU4MyVBMiVFMyU4MyU4MyVFMyU4MiVBRiVFMyU4MiVBMiVFMyU4MyU4MyVFMyU4MyU5NyVFMyU4MiU5MiVFNCVCRCU5QyVFMyU4MiU4QiZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9NzFhZmY2MTVkNjY1OTk1ZmIyMjdmOTkyMDQxN2MxYTE&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwZnV0b2FzZSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9YjY4ODc5Y2Q1ODI0Njk4NWQ3MjQ2NmQ0Y2MwMTM3YmU&blend-x=142&blend-y=491&blend-mode=normal&s=ff92841fa32db98212f0b66e78c35823')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">【個人メモ】JSON Serverを使って手っ取り早くWebAPIのモックアップを作る - Qiita</h1>
		<p class="rich-link-card-description">
		JSON Server いつだったか、mizchiさんがTwitterに書いていた JSON Serverというものを思い出した。  jsonファイルを用意しておけばAPIのリクエストを受け取り、また返してくれる APIのモ...
		</p>
		<p class="rich-link-href">
		https://qiita.com/futoase/items/2859a60c8b240da70572
		</p>
	</div>
</a></div>



```shell
$ npm -g i josn-server
```

db.json を作る。

```json:db.
{
  "users": [
    {
      "id": 1,
      "name": "futoase"
    },
    {
      "id": 2,
      "name": "hogehoge"
    }
  ],
  "limit": 100
}
```

json サーバを起動。

```shel
$ json-server db.json

Error: Type of "limit" (number)  is not supported. Use objects or arrays of objects.

```

落ちる。

https://blog.andooown.dev/post/2016/05/json-server/


<div class="rich-link-card-container"><a class="rich-link-card" href="https://blog.andooown.dev/post/2016/05/json-server/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://blog.andooown.dev/images/logo.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">WebAPIのモックアップ作りにjson-serverがすごい便利だった【旧ブログ】 - andooown's blog</h1>
		<p class="rich-link-card-description">
		【この記事は旧ブログから移転したものです】 現在、Xamarinを使って学祭のアプリを開発中なのですが、サーバサイドとクライアントサイドで友人
		</p>
		<p class="rich-link-href">
		https://blog.andooown.dev/post/2016/05/json-server/
		</p>
	</div>
</a></div>

とりあえず limit を削ると動くらしい。

```json:db.json
{
  "users": [
    {
      "id": 1,
      "name": "futoase"
    },
    {
      "id": 2,
      "name": "hogehoge"
    }
  ]
}
```

```shell
$ json-server --watch db.json
```
動いた。

```shell
$ curl -i localhost:3000/users
X-Powered-By: Express
Vary: Origin, Accept-Encoding
Access-Control-Allow-Credentials: true
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
X-Content-Type-Options: nosniff
Content-Type: application/json; charset=utf-8
Content-Length: 91
ETag: W/"5b-RmJhLfIsN54rEkip/lyeTKmvhMM"
Date: Mon, 18 Apr 2022 01:33:20 GMT
Connection: keep-alive
Keep-Alive: timeout=5

[
  {
    "id": 1,
    "name": "futoase"
  },
  {
    "id": 2,
    "name": "hogehoge"
  }
]
```

JSON API ってJSON key でアクセスすると取れるものなのか。
key がなければ 404 で返ってくる。

因みに、curl -i はヘッダとレスポンス全部表示オプション。
