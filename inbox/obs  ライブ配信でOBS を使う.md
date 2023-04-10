---
type: note
---

#obs

---
2022-12-20  16:19

# obs  ライブ配信でOBS を使う


# OBSを独自ストリーミングする方法
<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.yukkuriikouze.com/2019/03/09/2403/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.yukkuriikouze.com/wp-content/uploads/2018/09/Open_Broadcaster_Software_Logo.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">OBSを独自ストリーミングする方法 | ゆっくり遅報</h1>
		<p class="rich-link-card-description">
		はじめに youtubeやTwitchを使わずに配信する方法です。 Discordのニトロとかを使わずに画面共有したい方向け？ 技術紹介 動画をストリーミングするためには UDP...
		</p>
		<p class="rich-link-href">
		https://www.yukkuriikouze.com/2019/03/09/2403/
		</p>
	</div>
</a></div>

![[Pasted image 20221219172818.png]]

Nginx のモジュール込みのコンパイル。
rtmp と hls は同じモジュールのようだ。


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


encoder では、Mac の場合 HD60 S+ か


# OBS の設定

![[Pasted image 20221220163900.png | 800]]

映像ビットレート : 3 Mbps
音声ビットレート : 128
エンコーダ : ソフトウェア(x264)
エンコードプリセット : superfast


### エンコーダ

![[IMG_0960のコピー.jpg]]

アップルTV から HDMI を出してエンコーダに入れる。
出力は USB で Mac に直に差した。

OSB でソースを選択すると、映像キャブチャデバイスでこれが選択できる。

![[Pasted image 20221220164313.png]]


![[Pasted image 20221220165039.png]]

設定の配信でストリームサーバのURLを設定する。
![[Pasted image 20221220165210.png]]

配信を開始する。

### VLC で確認する。

![[Pasted image 20221220165420.png]]

rtmpサーバのURLでストリームを開くと、

![[Pasted image 20221220165533.png]]

ネットワーク内では rtmp で配信されている。


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


# HTTP 配信を録画の出力にする

OBS 設定 -->出力

![[Pasted image 20221221181902.png]]

録画ファイルの先を Nginx のドキュメントルート配下。
録画フォーマットを m3u8

OBS 設定 --> 詳細

ファイル名書式設定 初期値 %CCYY-%MM-%DD %hh-%mm-%ss を名前にして、アクセス出来るようにする。

![[Pasted image 20221221182216.png]]


OBS を録画で動かして、そのファイルを HTML で見れるようにするようだ。

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



# HTTP 配信をOBS配信にする

OBS の設定-->配信
![[Pasted image 20221222102603.png]]


nginx で rtmp サーバを立てる。

/usr/local/etc/nginx/nginx.conf

```shell
rtmp {
   server {
      listen 1935;
      chunk_size 4096;
      access_log /usr/local/var/log/nginx/rtmp_access.log;
      application live {
         live on;
         record off;
         hls on;
         hls_type live;
         hls_path /usr/local/var/www/stream/;
         hls_fragment 1s;
      }
   }
}
```
これで、OBSで配信開始すると /usr/local/var/www/stream/ 配下に m3u8 ファイルと ts ファイルが投げ込まれる。
これを hls.js で受信できる。

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
    //    const videoSrc = 'hls/live.m3u8'
    const videoSrc = 'stream/stream_live.m3u8'
    if (Hls.isSupported()) {
        var hls = new Hls();
        hls.loadSource(videoSrc);
        hls.attachMedia(video);
    } else if (video.canPlayType('application/vnd.apple.mpegurl')) {
        video.src = videoSrc;
    }
  </script>
</body>
</html>
```

![[Pasted image 20221222103230.png]]

結局、HSL ファイルが置かれれば、http で配信されるとのことだった。

# HTTP 配信 HCL

nginx.conf
```
http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    tcp_nopush     on;
    keepalive_timeout  60s;

    server {
        listen       8080;
        server_name  _;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            #add_header 'Cache-Control' 'no-cache';
            # CORS setup
            add_header Access-Control-Allow-Origin      *;
            add_header Access-Control-Allow-Credentials true;
            add_header Access-Control-Allow-Methods     "POST, GET, OPTIONS";
            add_header Access-Control-Allow-Headers     "Origin, Authorization, Accept";

            #root   html;
            root   /usr/local/var/www;
            index  index.html index.htm;
        }

```


# 127.0.0.1 から IP で外からアクセス出来るようにするには

Symantec Endpoint Protection の邪魔を解除するだけだった....orz


![[スクリーンショット 2022-12-22 11.13.01.png]]

はぁ~~




# 簡易ライブ配信システム



![[Pasted image 20221222143853.png]]


