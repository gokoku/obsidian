---
type: note
---

#nginx #osx

---
2022-08-04  09:44

# nginx を mac に立ててみる

https://blog.mothule.com/web/nginx/web-nginx-getting-started-step1-on-mac

```shell
$ brew install nginx

$ nginx -v
nginx version: nginx/1.23.1

$ brew services start nginx

# ポート確認
$ lsof -c nginx -P | grep LISTEN

8080 で LISTEN してる
```

http://people2.local:8080

![[Pasted image 20220804094840.png]]


## Document folder に HTML を置く

`/usr/local/etc/nginx/nginx.conf`

```
http {
  server {
    location / {
	  root html;
	  index index.html index.htm;
	  
    }
  }
}
```
ドキュメントの場所が書いてない?!

デフォルトの html は何?

`/usr/local/Cellar/nginx/1.23.1/html` というソフトリンクが 設定されてた。
これを参照してた訳だった。

```
/usr/local/var/www  <-- ここ
```

じゃあ、ここに置くか。
ただの html だし。

## ルートに置かないと動かなかったのでVirtual Host 立てた

```
/usr/local/etc/nginx/nginx.conf



http {
  server {
    listen 80;
    server_name localhost;

    location / {
          root html;
          index index.html index.htm;

    }
  }
  server {
    listen 80;
    server_name draw-tool;
    location / {
      root /usr/local/vsr/www/draw;
      index index.html
  }
}
```

クライアントの hosts を付け足す。

```
/etc/hosts

192.168.20.16 draw-tool;
```
![[Pasted image 20220804115004.png]]

Nginx のコンフィグをいじるときはシークレットモードでやったほうが良さげ。
キャッシュがあってよくわからなくなる。


