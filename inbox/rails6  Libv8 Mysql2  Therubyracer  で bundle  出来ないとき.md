#ruby/rails 

## libv8 エラーは brew で古いやつ入れて、それを使えようにするらしい。
```shell
    $ brew install v8-315
    $ bundle config --local build.libv8 --with-system-v8
```

## TherubyRacer エラーもそれを使う

```shell
    $ bundle config --local build.therubyracer --with-v80-dir=$(brew pprefix v8-315)
```
## Mysql2 は２つあるので、直に書く
```shell
    $ bundle config --local build.mysql2 "--with-ldfralgs=-L/usr/local/opt/openssl@1.1/lib"

openssl のパス情報は

    $ brew info openssl
```
.bundle/config を開いて追加する。
後の方で上書きされるので、

```shell
    BUNDLE_BUILD__MYSQL2: "--with-ldflags=-L/usr/local/opt/openssl@1.1/lib --with-cppflags=-I/usr/local/opt/openssl@1.1/include"
```

### MySQL クライアントがなかったので入らなかった

Docker で MySQL を立ててるので、Mac にはサーバーを入れてないし入れたくない。
なので、mysql client だけインストールする。

```shell
$ brew install mysql-client
```

.zshrc
```shell
export PATH="/usr/local/opt/mysql-client/bin:$PATH"
```

```shell
$ mysql -uroot -p -h127.0.0.1
Enter password:

mysql>
```
