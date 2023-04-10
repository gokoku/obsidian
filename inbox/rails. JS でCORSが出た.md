---
type: note
---

#rails

---
2022-12-09  11:26

# rails. JS でCORSが出た

```js
canvas.toDataURL('image/png', 1.0)
```

ここで、canvas が汚染されてるかもとエラーが出た。
CORS だ。

canvas に drawImage する画像の元が S3 から取ってきてるからのようだ。
なのでまず、追加するURLで引っ張る画像に crossOrigin = "Anonymous" をつけた。

```js
    const background = new Image()
    background.src = "<%= image_path('templates/template-a01/base.png') %>"
    // CORC 対策
    background.crossOrigin = "Anonymous"
    background.onload = () => {
      context.drawImage(background, 160, 100, background.width, background.height)
      resolve()
    }
```

これでエラーが変わった。

3:1 Access to image at 'https://s3.ap-northeast-1.amazonaws.com/ogasta/uploads/facilities/1/photos/1136/file/857506887638ace5af0ce8.jpeg?X-Amz-Expires=600&X-Amz-Date=20221209T023121Z&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIA5I6IBV5MHQX4MO5O%2F20221209%2Fap-northeast-1%2Fs3%2Faws4_request&X-Amz-SignedHeaders=host&X-Amz-Signature=3e7b8eb4153aa96dd1af54e8cff98a94b70eaf27a6260931f6826e8d989bef43' from origin 'https://ogasta.pmorioka.com' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.

S3 の header に Access-Control-Allow-Origin を付けるといいらしい。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://aws.amazon.com/jp/premiumsupport/knowledge-center/s3-configure-cors/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://a0.awsstatic.com/libra-css/images/logos/aws_logo_smile_1200x630.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Amazon S3 で CORS を設定して確認する</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://aws.amazon.com/jp/premiumsupport/knowledge-center/s3-configure-cors/
		</p>
	</div>
</a></div>



バケットを選んで

![[Pasted image 20221209113928.png]]

アクセス許可の下の方に Cross-Origin Resource Sharing 設定があるので、書き込んだ。

![[Pasted image 20221209115150.png | 500]]

```json
[
    {
        "AllowedHeaders": [],
        "AllowedMethods": [
            "GET"
        ],
        "AllowedOrigins": [
            "https://ogasta.pmorioka.com"
        ],
        "ExposeHeaders": [
            "Access-Control-Allow-Origin"
        ]
    }
]
```

上手く行った。

## curl で確認出来る

```shell
$  curl -i "https://s3.ap-northeast-1.amazonaws.com/ogasta/uploads/facilities/1/photos/1136/file/thumb_857506887638ace5af0ce8.jpeg?X-Amz-Expires=600&X-Amz-Date=20221209T024649Z&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIA5I6IBV5MHQX4MO5O%2F20221209%2Fap-northeast-1%2Fs3%2Faws4_request&X-Amz-SignedHeaders=host&X-Amz-Signature=4c3438fb7f07de9ca2a81e3862149cf2e3d95eeaca7e42f14d3653fb86539525" -H "Origin: https://ogasta.pmorioka.com"
HTTP/1.1 200 OK
x-amz-id-2: uWaTWtEozTK63qS3zP6B0TJ37HX1wiQhqdvd9IYH8zGFHjOQpr5d2natefs7tOsT73IlX58SWqw=
x-amz-request-id: A21GJH0ZE5247K3R
Date: Fri, 09 Dec 2022 02:54:18 GMT
Access-Control-Allow-Origin: https://ogasta.pmorioka.com
Access-Control-Allow-Methods: GET
Access-Control-Expose-Headers: Access-Control-Allow-Origin
Access-Control-Allow-Credentials: true
Vary: Origin, Access-Control-Request-Headers, Access-Control-Request-Method
Last-Modified: Thu, 08 Dec 2022 07:54:15 GMT
ETag: "06b15c05b00362307c9832f4cdee5912"
Accept-Ranges: bytes
Content-Type: image/jpeg
Server: AmazonS3
Content-Length: 8856
```

Access-Control-Expose-Headers: Access-Control-Allow-Origin とちゃんと応答してますね。
ところで、この curl -H はどんな風に繋がってるのかイマイチわからん。

-H は Header を追加するやつだった、Origin : https://ogasta.pmorioka.com を付加してs3リソースの Response Header Body を出力する。

