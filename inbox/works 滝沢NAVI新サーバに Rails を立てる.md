#ubuntu #works #ruby/rails 

---
2022-04-20  13:28

# ubuntu 滝沢NAVI新サーバに Rails を立てる
## bash にする
今のshell は sh なので、せめて bash にする。

```shell
$ chsh
パスワード:
takizawausr のログインシェルを変更中
新しい値を入力してください。標準設定値を使うならリターンを押してください
	ログインシェル [/bin/sh]: /usr/bin/bash

# 前準備
$ sudo apt update
$ sudo apt upgrade
```

## tmux を入れる
install 作業途中で通信が切れても処理が継続されるように tmux を入れてこれ上で作業をする。

```shell
$ sudo apt install tmux
```
コピーモードを設定する。
[[command  Tmux のコピーモードを使いこなしたい]]

## Linuxbrew のインストール
```shell
$sudo apt install build-essential procps curl file git

$/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"


$ test -d ~/.linuxbrew && eval "$(~/.linuxbrew/bin/brew shellenv)"

$ test -d /home/linuxbrew/.linuxbrew && eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"

$ test -r ~/.bash_profile && echo "eval \"\$($(brew --prefix)/bin/brew shellenv)\"" >> ~/.bash_profile

$ echo "eval \"\$($(brew --prefix)/bin/brew shellenv)\"" >> ~/.profile$
```


```shell
$ brew update
$ brew --version
Homebrew 3.4.7
```

## rbenv のインストール
```shell
$ brew install rbenv ruby-build

# new version にした時も、gem が用意されるようにする
$ git clone https://github.com/sstephenson/rbenv-default-gems.git ~/.rbenv/plugins/rbenv-default-gems
```



## ruby 2.7.4 のインストール
滝沢NAVI でのバージョンが 2.7.4

```shell
$ sudo apt install -y zlib1g-dev

$ rbenv install 2.7.4
```

## 滝沢NAVI のセッティング

```shell
$ cd /data/www/takizawanavi.jp/v5/   # <--ここにgit pull している
```

下準備。
一つ一つ入れないと、**"jp.archive.ubuntu.com:http へ接続できません"**
が出る。
タイムアウトする。
この時、```--fix-missing``` オプションを付けるとうまくいく。

```shell
$ bundle install
```

まず、sqlite3 で失敗する。

```shell
$ sudo apt install sqlite3
$ sudo apt install libsqlite3-dev

$ bundle install  # sqlite3 通った
```

RMagic で失敗する。
```shell
$ sudo apt install imagemagick
$ sudo apt install libgraphicsmagick1-dev
$ sudo apt install graphicsmagick-libmagick-dev-compat
$ sudo apt install libmagickcore-6.q16-dev
$ sudo apt-get install -y libmagickcore-dev
```

それでも失敗する。

```shell
# No package 'MagickCore' found とある。

$ find /usr -name "MagickCore"
./lib/x86_64-linux-gnu/pkgconfig/MagickCore.pc

# ここにある
$ export PKG_CONFIG_PATH='/usr/lib/x86_64-linux-gnu/pkgconfig'

$ bundle install # RMagic 通った
```

mysql2 で失敗する。
```shell
$ sudo apt install libmysqlclient-dev
# libmysqld-dev はこれで置き換えられるとのこと
```

```shell
$ bundle exec rails db:migrate RAILS_ENV=production
$ bundle exec rails db:seed RAILS_ENV=production
$ bundle exec rails assets:precompile RAILS_ENV=production
```


## Passenger for Nginx のインストール

公式サイトから


<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.phusionpassenger.com/docs/advanced_guides/install_and_upgrade/nginx/install/oss/focal.html" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.phusionpassenger.com/favicon.ico')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Installing Passenger - Passenger Library</h1>
		<p class="rich-link-card-description">
		
		</p>
		<p class="rich-link-href">
		https://www.phusionpassenger.com/docs/advanced_guides/install_and_upgrade/nginx/install/oss/focal.html
		</p>
	</div>
</a></div>

#### install Passenger packages

APT リポジトリに passenger のリポジトリを追加する。
そのための署名関係、PGP key とか HTTPS とか...?
そして passenger パッケージをインストール。

```shell
$ sudo apt-get install -y dirmngr gnupg
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
$ sudo apt-get install -y apt-transport-https ca-certificates

$ sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger focal main > /etc/apt/sources.list.d/passenger.list'

$ sudo apt update

$ sudo apt install -y libnginx-mod-http-passenger
```

#### enabled the Passenger Nginx module and restart Nginx
Passenger Nginx module を enable にして restart Nginx

```shell
$ if [ ! -f /etc/nginx/modules-enabled/50-mod-http-passenger.conf ]; then sudo ln -s /usr/share/nginx/modules-available/mod-http-passenger.load /etc/nginx/modules-enabled/50-mod-http-passenger.conf ; fi


$ sudo ls /etc/nginx/conf.d/mod-http-passenger.conf
```

/etc/nginx/modules-enabled/ にモジュールがリンクされていればいいってことだ。
見てみるとすでにソフトリンクがこのファイルと張られてるので叩かない。

ls で確認するだけで良いようだ。

Nginx の再起動。

```shell
$ sudo service nginx restart
```

#### check installation

```shell
$ sudo passenger-config validate-install
```

![[Pasted image 20220420172327.png]]

 Passenger itself で Enter。
 
![[Pasted image 20220420172421.png]]

これでOKらしい。
プロセス開始の確認。

```shell
$ sudo passenger-memory-stats
```

![[Pasted image 20220420172734.png]]

なんだかいいらしい。

#### update regularly
```shell
$ sudo apt update
$ sudo apt upgrade
```

## Passenger と Rails を結ぶ


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/1LDK/items/7b34c5d5d94e9aa9f7f9" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9VWJ1bnR1MjAuMDQlMjBMVFMlRTMlODElQUJuZ2lueCVFMyU4MCU4MVJhaWxzJUUzJTgyJUEyJUUzJTgzJTk3JUUzJTgzJUFBJUUzJTgyJUIxJUUzJTgzJUJDJUUzJTgyJUI3JUUzJTgzJUE3JUUzJTgzJUIzJUUzJTgyJTkyJUU5JTg1JThEJUU3JUJEJUFFJUUzJTgxJTk5JUUzJTgyJThCJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz02NjIzYTA4YWE1YzJjMWM5NmEyMjljMDA2OGFhMDY5OQ&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwMUxESyZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9OTRjMzc2ZWY3MzY3NDYwNTBkZDY0YzgyZTk5ZTRlMDk&blend-x=142&blend-y=491&blend-mode=normal&s=dace6607fef02a31a7d98cd5a14580fb')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Ubuntu20.04 LTSにnginx、Railsアプリケーションを配置する - Qiita</h1>
		<p class="rich-link-card-description">
		環境  Ubuntu20.04 LTS git version 2.25.1 MySQL version 8.0.23 rbenvのインストール terminal $ sudo apt update $ sudo apt...
		</p>
		<p class="rich-link-href">
		https://qiita.com/1LDK/items/7b34c5d5d94e9aa9f7f9
		</p>
	</div>
</a></div>

すでにセットしてあるNginx のコンフィグをいじる。

/etc/nginx/sites-enabled/takizawanavi.jp

```config
server {

        root /data/www/takizawanavi.jp/v5/public;
        rails_env       production;
        passenger_enabled       on;

        index index.html index.htm index.nginx-debian.html;
        server_name takizawanavi.jp www.takizawanavi.jp;

        location / {
                try_files $uri $uri/ =404;
        }
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
```

確認。

```shell
$ sudo service nginx testconfig
ok
```

リスタート。

```shell
$ sudo service nginx restart
```

![[Pasted image 20220420175709.png | 500]]

## デバッグ
```
$ bundle exec rails assets:precompile

ExecJS::RuntimeUnavailable: Could not find a JavaScript runtime.
```

#### Nodenv install
```shell
$ brew install nodenv
```

~/.bash_profile に追加。
```shell
eval "$(nodenv init -)"
```

```shell
$ . ~/.bash_profile
$ nodenv --version
$ npm -g i yarn

# 昨日通らなかった Asset compile
$ bundle exec rails assets:precompile RAILS_ENV=production
OK!!

# nginx 再起動
```
ゴニョゴニョしてるうちに passenger が Rails を認識しなくなった。
/tmp にある nginx のディレクトリ clean しちゃったせいかな。

```shell
$ sudo passenger-config restart-app
Phusion Passenger(R) is currently not serving any applications.
```
プロセスはある。
```shell
$ sudo passenger-memory-stats
```
```shell
$ sudo passenger-status
Version : 6.0.13
Date    : 2022-04-21 18:18:38 +0900
Instance: lvZfG0kd (nginx/1.18.0 Phusion_Passenger/6.0.13)

----------- General information -----------
Max pool size : 6
App groups    : 0
Processes     : 0
Requests in top-level queue : 0

----------- Application groups -----------
```
無い。

因みに nginx のシンタックスチェックは
```shell
$ sudo nginx -t
```

- 次の日見たらrails アプリを見つけていた
```shell
$ sudo passenger-config restart-app

Please select the application to restart.
Tip: re-run this command with --help to learn how to automate it.
If the menu doesn't display correctly, press '!'

 ‣   /data/www/takizawanavi.jp/v5 (production)
     Cancel

Restarting /data/www/takizawanavi.jp/v5 (production)

```
結果オーライ? 昨日は reboot までしたのになー。
どうも、一定期間リクエストがなかったのでインスタンスを終了させたっぽい。
めちゃ、正常動作してるじゃん。

いじってるうちに消えた。
```shell
$ sudo passenger-confg restart-app
*** Cleaning stale instance directory /tmp/passenger.42655ST
Phusion Passenger(R) is currently not serving any applications.
```

戻った。
なんだか nginx の設定書き換えの失敗でそうなるようだ。
失敗した後、nginx を再起動して、get リクエストすると、Passenger が App を見つけてくれる。

- passenger_app_root を設定するとダメになった。

- Nginx のログを見る
/var/log/nginx/takizawanavi.jp/error.log
```log
Security update check: no update found (next check in 24 hours)
```
このような奴が出てる。

passenger-config restart-app を駆けるとこれが出る。

```shell
Checking whether to disconnect long-running connections │
for process 102549, application /data/www/takizawanavi.jp/v5 (production)
```

-  しかし、Nginx は Not Found と普通に返す


[[works 滝沢NAVI新サーバに Rails を立てる デバッグ編]]
