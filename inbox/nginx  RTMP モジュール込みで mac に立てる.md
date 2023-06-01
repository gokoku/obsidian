---
type: note
---

#nginx 

---
2022-12-20  14:48

# nginx  RTMP モジュール込みで mac に立てる

https://kannokanno.hatenablog.com/entry/2014/02/10/134920
<div class="rich-link-card-container"><a class="rich-link-card" href="https://kannokanno.hatenablog.com/entry/2014/02/10/134920" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://hatenablog-parts.com/embed?url=https%3A%2F%2Fkannokanno.hatenablog.com%2Fentry%2F2014%2F02%2F10%2F134920')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Mac - homebrewでnginxを入れるときはnginx-fullを入れよう - ぼっち勉強会</h1>
		<p class="rich-link-card-description">
		nginxではなくてnginx-fullを使う homebrewではbrew install nginxとすることでnginxを入れることが出来ますが、 この標準Formulaはオプションが最低限のものしかありません。 /Users/kanno% brew options nginx --with-debug Comp…
		</p>
		<p class="rich-link-href">
		https://kannokanno.hatenablog.com/entry/2014/02/10/134920
		</p>
	</div>
</a></div>


homebrew で nginx を入れる時は nginx-full を tap するといいとのこと。

普通の nginx の options は標準最低限らしい。
でも、nginx-full にはたくさんあって、この中に動画配信モジュールもある。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/kiyu_knowledges/items/062ff23d334a24c4efec" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9TWFjJUUzJTgxJUE3bmdpbnglRTMlODIlOTIlRTQlQkQlQkYlRTMlODElQTMlRTMlODElQTYlRTclOTQlOUYlRTQlQjglQUQlRTclQjYlOTklRTMlODIlQjclRTMlODIlQjklRTMlODMlODYlRTMlODMlQTAlRTMlODIlOTIlRTYlQTclOEIlRTclQUYlODklRTMlODElOTklRTMlODIlOEImdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT01NiZ0eHQtY2xpcD1lbGxpcHNpcyZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPWE2OGJhNTg1ODU0MmQ5ZWRjY2ExOTc0ZDYxOGI1NzBm&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwa2l5dV9rbm93bGVkZ2VzJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz1mMzY0NjY3YzUzMjZhN2NhZWU5ODUwYjU0N2FhMDAyYg&blend-x=142&blend-y=491&blend-mode=normal&s=5aa26a35da7a71f7d589360f6eede03d')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Macでnginxを使って生中継システムを構築する - Qiita</h1>
		<p class="rich-link-card-description">
		生中継がしたい！ ということで、何も知らないところから開発を始める。 nginxのインストール  nginxをrtmp-module付きでインストール 普通にやるとエラーが出る  $ brew install nginx-full...
		</p>
		<p class="rich-link-href">
		https://qiita.com/kiyu_knowledges/items/062ff23d334a24c4efec
		</p>
	</div>
</a></div>

https://qiita.com/hagane5563/items/842afe6d6e7100db3a28
<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/hagane5563/items/842afe6d6e7100db3a28" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9bmdpbnglRTMlODElQTclRTMlODIlQjklRTMlODMlODglRTMlODMlQUElRTMlODMlQkMlRTMlODMlOUYlRTMlODMlQjMlRTMlODIlQjAlRTMlODIlQjUlRTMlODMlQkMlRTMlODMlOTAlRTMlODIlOTIlRTclQUIlOEIlRTMlODElQTYlRTMlODElQTYlRTMlODMlQTklRTMlODIlQTQlRTMlODMlOTYlRTklODUlOEQlRTQlQkYlQTElRTMlODElOTklRTMlODIlOEIlRTMlODAlODImdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT01NiZ0eHQtY2xpcD1lbGxpcHNpcyZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTE3NDMyNDkwNWRjM2ZkMDg4YWY3MjU1MGUwZjA4MDQw&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwaGFnYW5lNTU2MyZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9MjcwMzhmOTNmYWZiZTEyZDAwOTFjMDY1ZWQ2ZGRkNWE&blend-x=142&blend-y=491&blend-mode=normal&s=27f6605493142bbdb325aa18b49961df')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">nginxでストリーミングサーバを立ててライブ配信する。 - Qiita</h1>
		<p class="rich-link-card-description">
		この記事はnginxアドベントカレンダーの22日の記事です。  nginxを利用してストリーミングサーバを構築したいと思います。  今回のテスト環境： さくらのクラウド 1 GB / 1 仮想コア 20 GB SSDプラン OS:Ce...
		</p>
		<p class="rich-link-href">
		https://qiita.com/hagane5563/items/842afe6d6e7100db3a28
		</p>
	</div>
</a></div>



```shell
$ brew tap marcqualie/nginx
$ brew options nginx-full

$ brew install nginx-full --with-rtmp-module
```
```shell
- Tips -
Run port 80:
 $ sudo chown root:wheel /usr/local/opt/nginx-full/bin/nginx
 $ sudo chmod u+s /usr/local/opt/nginx-full/bin/nginx
 
Reload config:
 $ nginx -s reload
 
Reopen Logfile:
 $ nginx -s reopen
 
Stop process:
 $ nginx -s stop
 
Waiting on exit process
 $ nginx -s quit
```

## config の設定

/usr/local/etc/nginx/nginx.conf

```shell
http {
	server {
		listen 80;
		server_name localhost;
		#charset koi8-r;
		...
	}
}

rtmp {
   server {
      listen 1935;
      application live {
         live on;
         hls on;
         record off;
         hls_path /usr/local/Cellar/nginx-full/1.23.1/html;

         hls_fragment 1s;

         hls_type live;
      }
   }
}
```

start
```shell
$ sudo chown root:wheel /usr/local/opt/nginx-full/bin/nginx
$ sudo chmod u+s /usr/local/opt/nginx-full/bin/nginx

$ sudo nginx

ストップ
$ sudo nginx -s stop
```

# ファイル設置



IP で他のマシンでは取得に失敗する。
```
VIDEOJS: ERROR: (CODE:4 MEDIA_ERR_SRC_NOT_SUPPORTED) The media could not be loaded, either because the server or network failed or because the format is not supported.
```

Nginx に CORS 対策をしてみるも、ここは突破できなかった。

```shell
    server {
        listen       80;
        server_name  localhost;

        add_header Access-Control-Allow-Origin      *;
        add_header Access-Control-Allow-Credentials true;
        add_header Access-Control-Allow-Methods     "POST, GET, OPTIONS";
        add_header Access-Control-Allow-Headers     "Authorization";
```


/usr/lcoal/etc/nginx/mime.types にこうある。
```shell
	application/vnd.apple.mpegurl     m3u8;
```

https://note.com/educator/n/nea0f4663a5ba
<div class="rich-link-card-container"><a class="rich-link-card" href="https://note.com/educator/n/nea0f4663a5ba" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://assets.st-note.com/production/uploads/images/80418300/rectangle_large_type_2_6e938a28d7d14af03b673a732bc4b4ec.png?fit=bounds&quality=85&width=1280')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">【2022年度版】今でも動くライブ配信システムの構築（rtmp、hls対応）とブラウザ視聴対応について｜e｜note</h1>
		<p class="rich-link-card-description">
		ここ1ヶ月は忙しくし過ぎていて、noteに投稿出来ていなかったが、ある出来事がきっかけで個人的な使用でライブ配信する方法を調べた。中々時間が掛かったが有意義な内容だと思うためnoteに投稿する。  前提  本件は、さくらのクラウドでubuntuサーバーに、nginx-rtmp-moduleをインストールして試した。 GitHub - arut/nginx-rtmp-module: NGINX-based Media Streaming Server NGINX-based Media Streaming Server. Contribute to arut/nginx-
		</p>
		<p class="rich-link-href">
		https://note.com/educator/n/nea0f4663a5ba
		</p>
	</div>
</a></div>


rtmp プロトコルは 2020 年で flash サポート終了でブラウザでは動かなくなった。


ubuntu でのインストールがある。
```shell
$ sudo apt install nginx libnginx-mod-rtmp
```

Filrewall の解放
```shell
$ sudo ufw allow 1935/tcp
```


ここが参考になった。
ライブラリでやっと動かせたのが hls.js だった。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.alpha.co.jp/blog/202201_02" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.alpha.co.jp/blog/static/032ebac5295df8da183cf3ab0b62b21c/90823/cover.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">HLSによる社内向け動画配信 - アルファテックブログ</h1>
		<p class="rich-link-card-description">
		OBS StudioとWebサーバーを使用して、外部サービスを利用せずに動画を生配信する方法を紹介します。
		</p>
		<p class="rich-link-href">
		https://www.alpha.co.jp/blog/202201_02
		</p>
	</div>
</a></div>

/usr/local/var/www/play.html
```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>
</head>
<body>
  <video id="video" autoplay controls height="540" width="960"></video>

  <script>
    const video = document.getElementById('video')
    const videoSrc = 'hls/live.m3u8'
    if (Hls.isSupported()) {
        var hls = new Hls();
        hls.loadSource(videoSrc);
        hls.attachMedia(video);
    }
    // HLS.js is not supported on platforms that do not have Media Source
    // Extensions (MSE) enabled.
    //
    // When the browser has built-in HLS support (check using `canPlayType`),
    // we can provide an HLS manifest (i.e. .m3u8 URL) directly to the video
    // element through the `src` property. This is using the built-in support
    // of the plain video element, without using HLS.js.
    //
    // Note: it would be more normal to wait on the 'canplay' event below however
    // on Safari (where you are most likely to find built-in HLS support) the
    // video.src URL must be on the user-driven white-list before a 'canplay'
    // event will be emitted; the last video event that can be reliably
    // listened-for when the URL is not on the white-list is 'loadedmetadata'.
    else if (video.canPlayType('application/vnd.apple.mpegurl')) {
        video.src = videoSrc;
    }
  </script>
</body>
</html>
```

flash がサポート終了が 2020 だから、結構最近まで色々動かせていたようだ。

localhost/play.html で動くが、外からが見られない。まるで Firewall が邪魔してるみたいだ。

*Semantice endpoint* が邪魔してた!!!

ちゃんと出来てる。

# 録画

Nginx に ffmpeg 変換スクリプトを仕込む。
flv じゃなくて hls から mp4 にしたい。
https://centossrv.com/nginx-rtmp-record.shtml

hls から mp4 変換。
https://shotaste.com/blog/convert-hls/


