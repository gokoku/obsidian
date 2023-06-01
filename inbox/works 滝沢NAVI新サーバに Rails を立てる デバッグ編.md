#works #ruby/rails #nginx

---
2022-04-22  16:38

# works 滝沢NAVI新サーバに Rails を立てる デバッグ編


## 簡単な rails app の developer mode
なんだかわからなくなったので、簡単な Rails app を本番サーバに立てて Nginx で動くか試してみる。

```shell
$ rails new sample
```

ここで、/etc/nginx/sites-available/takizawanavi.jp を切り替えてみる。
 簡単にするため development モードで動かす。

```config
server {
	root /data/www/takizawanavi.jp/sample/public;
	rails_env   development;
	passenger_enabled   on;
```
![[Pasted image 20220422164450.png |400]]

まず、出た。

```shell
$ cd sample
$ rails g scaffold Post title:string content:text
$ rails db:migrate

$ sudo service nginx restart
```
takizawanavi.jp/posts にアクセス。

![[Pasted image 20220422164744.png]]

ここで、/etc/nginx/sites-available/takizawanavi.jp のコメントアウトしてみる。

```config

server {

        root /data/www/takizawanavi.jp/sample/public;
        rails_env       development;
        passenger_enabled       on;
        index index.html index.htm index.nginx-debian.html;
        server_name takizawanavi.jp www.takizawanavi.jp;

#        location / {
#                try_files $uri $uri/ =404;
#        }

```
Nginx 再起動して、takizawanavi.jp/posts にアクセスしてみると!!
![[Pasted image 20220422165118.png]]

なんと!! 動いた！

nginx の error.log にこれがあるが、大丈夫なようだ。

```
[ N 2022-04-22 16:50:25.1774 117162/T6 age/Cor/SecurityUpdateChecker.h:519 ]: Security update check: no update found (next check in 24 hours)
```
ちゃんと Passenger と Nginx がやり取りできてるのが分かった。

## 簡単な rails app の production mode

![[Pasted image 20220422165548.png | 400]]

log/production.log をみる。

Credentials の作り方とかわからん。

[[rails  Credentials 周りの使い方]]

出来てしまった。

### 再び滝沢 NAVI

エラーでこれが出てくるのがダメ でもなかった。出たまま動いてるよ。

```log
[ N 2022-04-22 17:48:37.1196 122654/T6 age/Cor/SecurityUpdateChecker.h:519 ]: Security update check: no update found (next check in 24 hours)
```


# 翌週になったら動いた!

一旦 Task を走らせた。

```shell
$ rails posts:citizen_update
```

/etc/nginx/sites-available/takizawanavi.jp

```shell

server {

        root /data/www/takizawanavi.jp/v5/public;
        rails_env       production;
        passenger_enabled       on;


        index index.html index.htm index.nginx-debian.html;

        server_name takizawanavi.jp www.takizawanavi.jp;

#        location / {
#                try_files $uri $uri/ =404;
#        }

        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        }


    access_log /var/log/nginx/takizawanavi.jp/access.log;
    error_log /var/log/nginx/takizawanavi.jp/error.log;


    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/takizawanavi.jp/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/takizawanavi.jp/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

passenger_disable_security_update_check off;

server {
    if ($host = www.takizawanavi.jp) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = takizawanavi.jp) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


        listen 80;
        listen [::]:80;

        server_name takizawanavi.jp www.takizawanavi.jp;
    return 404; # managed by Certbot

}
```

コメントアウトした3行は大事。これのせいで、階層下に入れなかった。
実体のないファイルは 404 にして弾くようだ。

```shell
$ sudo service nginx restart
```
![[Pasted image 20220425152126.png]]

やりました。

passenger が設置してくれたファイル  mod-http-passenger.conf の
passenger_ruby を rbenv のグローバルにしても、デフォルトのままでもどちらでも動いた。


/etc/nginx/conf.d/mod-http-passenger.conf

```shell
### Begin automatically installed Phusion Passenger config snippet ###
passenger_root /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini;
#passenger_ruby /usr/bin/passenger_free_ruby;
passenger_ruby /data/www/takizawanavi.jp/.rbenv/shims/ruby;
### End automatically installed Phusion Passenger config snippet ###
```
ruby のバージョン変えた時に気をつけましょう。
free_ruby は 2.7 だった。
passenger は 6.0.13

```shell
$ sudo nginx -t   # シンタックスチェック
$ sudo service nginx restart
```

### passenger が変と思ったら大丈夫だった

nginx リスタート後、passenger が rails app を見つけられてなくてびっくりした。
でも rails は動いてる。

少し時間がかかってたみたい。
しばらくして打ち込んで見たらちゃんと出てた。
```shell
$ sudo passenger-config restart-app

Please select the application to restart.
Tip: re-run this command with --help to learn how to automate it.
If the menu doesn't display correctly, press '!'

 ‣   /data/www/takizawanavi.jp/v5 (production)
     Cancel

Restarting /data/www/takizawanavi.jp/v5 (production)
```


```shell
$ sudo passenger-memory-stats

Version: 6.0.13
Date   : 2022-04-25 16:48:00 +0900
------------- Apache processes -------------
*** WARNING: The Apache executable cannot be found.
Please set the APXS2 environment variable to your 'apxs2' executable's filename, or set the HTTPD environment variable to your 'httpd' or 'apache2' executable's filename.


----------- Nginx processes -----------
PID     PPID    VMSize   Private  Name
---------------------------------------
446199  1       66.3 MB  0.5 MB   nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
446200  446199  66.8 MB  1.1 MB   nginx: worker process
446201  446199  66.6 MB  0.7 MB   nginx: worker process
### Processes: 3
### Total private dirty RSS: 2.28 MB


------ Passenger processes -------
PID     VMSize     Private   Name
----------------------------------
446187  278.0 MB   1.9 MB    Passenger watchdog
446190  1026.3 MB  3.5 MB    Passenger core
446356  442.8 MB   138.5 MB  Passenger RubyApp: /data/www/takizawanavi.jp/v5 (production)
### Processes: 3
### Total private dirty RSS: 143.92 MB

```

ちゃんと Passenger processes があった!
