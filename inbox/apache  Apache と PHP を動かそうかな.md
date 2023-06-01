#apache #php #mac 

---
2022-02-19  20:39

# apache  Apache と PHP を動かそうかな

M1 で入れれるらしい。

https://kujirahand.com/blog/index.php?macOS%E3%81%A7apache%E3%81%A8php%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB

```shell
$ brew install httpd
$ brew install php
```

/usr/local/etc/httpd/httpd.conf
```config
Group staff

ServerName www.example.com:8080  # コメントインしただけ

DirectoryIndex index.html index.php


<IfModule mime_module>
   AddHndler application/x-httpd-php .php
</IfModule>

LoadModule php_module /usr/local/opt/php@8.1/lib/httpd/modules/libphp.so
```

### service について

service は Launchctl に load する plist をセットしたり削除する働きもする。

```shell
$ sudo brew services start httpd

$ sudo brew services stop httpd

これは　LaunchAgents に記述ファイルが生成されたり、削除したりしてた。
つまり、Laounchctl load で読み込まれるかどうかだった。

$ sudo brew services info httpd
Running x
Loading x
Schedulable: x
```

ワーニング消すために、chown george:admin ... で root から george にした。
sudo つけるとワーニングでて、chown して動かしてとか言われる。

sudo つけた時のroot ユーザの設定はどこに書かれるのかな??
sudo つけると、/Library/LaunchDaemons/homebrew.mxcl.httpd.plist
が生成されたり、消えたりする。

sudo つけないで、自分ユーザでうごかすと、
~/Library/LaunchAgents/homebrew.mxcl.httpd.plist が現れる。

ということで、
```shell
$ brew services start httpd
$ brew services stop httpd
$ brew services info httpd

で行こう。としたら、これでは動かなかった。Running に失敗してる。

$ brew services list

httpd      error  256 george ~/Library/LaunchAgents/homebrew.mxcl.httpd.plist

```

よくわからんが、sudo つけて service するとオーナー変えてユーザで動かせと言っているが、そのようにして sudo 付けないと起動に失敗する。

service として使いたかったら注意されても sudo で動かす。

あるいは、`sudo apachectl -k start` で直に動かす事にする。
```shell
$ sudo apachectl -k start
$ sudo apachectl -t
$ sudo apachectl -k stop
$ sudo apachectl -k restart

$ brew install lynx
$ sudo apachectl status
```

-k は 停止、再起動を指定するとのこと


### デフォルトはポート8080

設定がしてあった。
```config
# /usr/local/etchttpd/httpd.conf

Listen 8080
```



Document root --> /usr/local/var/www/

info.php を置くと動いてくれたので、asdf の php は remove した。

```shell
$ which php
/usr/local/bin/php
```

## cakePHP

```shell
$ brew install composer

$ composer create-project --prefer-dist cakephp/app:~4.0 sample

$ cd sample
```

あれ? ローカルサーバで出来る。

```shell
$ bin/cake server
```

http://localhost:8765

じゃあいいや。

[[cakephp  CakePHP 入門]]