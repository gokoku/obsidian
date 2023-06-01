---
type: note
---

#vscode 

---
2023-01-18  21:12

# vscode  API をサクッと叩くREST Client Extention

![[Pasted image 20230118211307.png]]

これ以上簡単なものないってくらいすごいぞ!!

https://qiita.com/yoshii0110/items/d40a1c8bcf0353b5bff3
<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/yoshii0110/items/d40a1c8bcf0353b5bff3" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9JUUzJTgyJUI1JUUzJTgyJUFGJUUzJTgzJTgzJUUzJTgxJUE4QVBJJUUzJTgyJTkyJUU1JThGJUE5JUUzJTgxJThGJUU2JTk2JUI5JUU2JUIzJTk1JTIwJTI4VlMlMjBDb2RlJUUzJTgxJUE3SFRUUCVFMyU4MyVBQSVFMyU4MiVBRiVFMyU4MiVBOCVFMyU4MiVCOSVFMyU4MyU4OCVFMyU4MSU4QyVFOSU4MCU4MSVFMyU4MiU4QyVFMyU4MSVBMSVFMyU4MiU4MyVFMyU4MSU4NiVFNiU4QiVBMSVFNSVCQyVCNSVFNiVBOSU5RiVFOCU4MyVCRCUyOSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9ZWFmOTg5NGU4M2JiMjhhM2RmNDUyNjg5MTY4ZTY2ZmE&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQweW9zaGlpMDExMCZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9N2RmYTIxY2ZlNmNjM2U2MGE1ZGE0YzFjM2M5MGZjNTg&blend-x=142&blend-y=491&blend-mode=normal&s=5e7a1e6ba3c51742c5178f7bc7aeb605')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">サクッとAPIを叩く方法 (VS CodeでHTTPリクエストが送れちゃう拡張機能) - Qiita</h1>
		<p class="rich-link-card-description">
		概要  開発中のAPIを試したり、サードパーティのAPIをサクッと叩いてみたいといった時に皆さんどのようにしますか？ 私は、curlコマンドやPostmanをよく使っています。 ただ、もっと楽にHTTPリクエストを投げ、かつその時使...
		</p>
		<p class="rich-link-href">
		https://qiita.com/yoshii0110/items/d40a1c8bcf0353b5bff3
		</p>
	</div>
</a></div>


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/toshi0607/items/c4440d3fbfa72eac840c" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9VlMlMjBDb2RlJUU0JUI4JThBJUUzJTgxJUE3SFRUUCVFMyU4MyVBQSVFMyU4MiVBRiVFMyU4MiVBOCVFMyU4MiVCOSVFMyU4MyU4OCVFMyU4MiU5MiVFOSU4MCU4MSVFNCVCRiVBMSVFMyU4MSU5NyVFMyU4MCU4MVZTJTIwQ29kZSVFNCVCOCU4QSVFMyU4MSVBNyVFMyU4MyVBQyVFMyU4MiVCOSVFMyU4MyU5RCVFMyU4MyVCMyVFMyU4MiVCOSVFMyU4MiU5MiVFNyVBMiVCQSVFOCVBQSU4RCVFMyU4MSVBNyVFMyU4MSU4RCVFMyU4MiU4QiVFMyU4MCU4Q1JFU1QlMjBDbGllbnQlRTMlODAlOEQlRTYlOEIlQTElRTUlQkMlQjUlRTMlODElQUUlRTclQjQlQjklRTQlQkIlOEImdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT01NiZ0eHQtY2xpcD1lbGxpcHNpcyZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPWI0MTdlYjZjMjRjMTg0YzEwZTNmODczMDA2YzkzMTBj&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwdG9zaGkwNjA3JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz05M2U0N2FmZWMyYjVkMTI1ZGQxYzJkNWYwYjhiZjRlOQ&blend-x=142&blend-y=491&blend-mode=normal&s=992e11f1b902d2baaaa2f62a5718e8ec')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">VS Code上でHTTPリクエストを送信し、VS Code上でレスポンスを確認できる「REST Client」拡張の紹介 - Qiita</h1>
		<p class="rich-link-card-description">
		概要 Visual Studio Code（以下VS Code）の拡張機能であるREST Clientが便利だったのでその紹介です。 使い方を文字とgifで説明していきます。 説明はマーケットプレース以上の情報を足していないの...
		</p>
		<p class="rich-link-href">
		https://qiita.com/toshi0607/items/c4440d3fbfa72eac840c
		</p>
	</div>
</a></div>



適当なファイルに記述。

```###``` と ```###``` でリクエストを区別する。

## GET
```
GET https://reqres.in/api/users?page=2 HTTP/1.1
Content-Type application/json

###

GET http://localhost:8000/api/posts
?user_id=1
&title=hogehoge
&body=fugafuga
```
カーソルのあるリクエストを投げる。
F1 + Rest Client: Send Request
を実行すると、

右ペインに response

![[Pasted image 20230118211747.png]]
```
GET https://zipcloud.ibsnet.co.jp/api/search
?zipcode=0200611
```

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Content-Type: text/plain;charset=utf-8
X-Cloud-Trace-Context: eff1a4d86805c80736eb054fc3fed15f
Date: Wed, 18 Jan 2023 12:44:17 GMT
Server: Google Frontend
Content-Length: 277
Connection: close

{
  "message": null,
  "results": [
    {
      "address1": "岩手県",
      "address2": "滝沢市",
      "address3": "巣子",
      "kana1": "ｲﾜﾃｹﾝ",
      "kana2": "ﾀｷｻﾞﾜｼ",
      "kana3": "ｽｺﾞ",
      "prefcode": "3",
      "zipcode": "0200611"
    }
  ],
  "status": 200
}
```


## POST で json を投げる

```

POST https://reqres.in/api/users HTTP/1.1
Content-Type: application/json

{
  "name": "morpheus",
  "job": "leader"
}
```





