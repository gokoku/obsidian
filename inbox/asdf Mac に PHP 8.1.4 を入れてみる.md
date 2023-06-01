#asdf #php

---
2022-02-18  20:55

# asdf PHP 8.1.4 を入れてみる

```shell
$ asdf list all php

$ brew install autoconf
$ brew install bison
$ brew install re2c
$ brew install libgd
$ brew install libiconv
$ brew install oniguruma
$ brew install libzip

$ asdf install php 8.1.3
$ asdf global php 8.1.3
$ php -v

$ asdf install php 7.4.28
$ asdf global php 7.4.28
$ php -v

```

montoray は Apache があるが、php はない。

コメントイン。

```
# /etc/apache/httpd.conf

LoadModule rewrite_module libexec/apache2/mod_rewrite.so
```

さて、libphp.so ファイルってどうやって生成するのかな??

