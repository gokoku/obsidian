#ruby/rails 



Apache と Rack のバランスを取れるようにする。

## Rack サイド

Rack では、RAILS_RELATIVE_URL_ROOT 環境変数を取り込んでルートをマッピングさせる。

```rb
require_relative 'config/environment'
map Rails.application.config.relative_url_root || "/" do
  run Rails.application
end
```

## Apache サイド

ドキュメントルート html、
そこにソフトリンクで Rails の Public を当てる。
サブディレクトリ名は demo にする。

```shell
$ cd /extdisk/vhosts/rabipeoca.jp/html
$ ln -s /extdisk/vhosts/rabipeoca.jp/rabipeoca-rails/public demo
```

```xml
<IfModule mod_ssl.c>
<VirtualHost 10.31.0.2:443>
  ServerAdmin sup@people.co.jp
  ServerName rabipeoca.jp
  DocumentRoot /extdisk/vhosts/rabipeoca.jp/html

  <Directory /extdisk/vhosts/rabipeoca.jp/html>
     AllowOverride all
     Require all granted
     Options -MultiViews
  </Directory>

  <Directory /extdisk/vhosts/rabipeoca.jp/rabipeoca-rails/public>
     AllowOverride all
     Options FollowSymLinks ExecCGI
     Require all granted
     Options -MultiViews
  </Directory>


  <Location /demo>
     PassengerAppRoot /extdisk/vhosts/rabipeoca.jp/rabipeoca-rails
     RackEnv development
#     RackEnv production
     RackBaseURI /
     PassengerBaseURI /
#     PassengerFriendlyErrorPages off
  </Location>
...

```

Directory ディレクションでファイル系を設定して、

Location ディレクションで URL 系を設定する感じ。

## 起動する

```shell
$ sudo apachectl -t                # チェックする。SSL Cert 絡みでは sudo を付けないとOKにならない
$ sudo systemctl restart httpd     # 最近 systemctl でまとめられているようだ
```

この時点では passenger は Rails アプリを感知出来てない。
いったん、URL を叩かないと気がついてくれないので注意。

```shell
$ sudo passenger-config restart-app
```
