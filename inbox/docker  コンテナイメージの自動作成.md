#docker 

## Dockerfile を作る

```shell
    $ mkdir build_http 
    $ cd build_http
```

```shell
FROM ubuntu:latest
MAINTAINER George Tada

RUN apt update && apt install -y apache2
ADD index.html /var/www/html/index.html
CMD apachectl start && bash
```

index.html を用意する。
```shell
    <h1>Hello, Dockerfile!</h1>
```
## Docker イメージを作る

Dockerfile のあるディレクトリにいるとして、

```shell
    $ docker build -t tada/httpd:ver1.1 .
```

イメージが作られた。

```shell
    $ docker images
    REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
    tada/httpd              ver1.1              ee30b1222a01        21 seconds ago      188MB
```

出来たイメージでコンテナを立てる。

```shell
    $ docker run -itd -p8000:80 --name web01 tada/httpd:ver1.1
    $ docker ps
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
    499d1b0c80ec        tada/httpd:ver1.1   "/bin/sh -c 'apachec…"   13 seconds ago      Up 12 seconds       0.0.0.0:8000->80/tcp   web01
```


# コンテナイメージの更新

出来た Docker イメージを見る。

```shell
    $ docker images
    REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
    tada/httpd              ver1.1              ee30b1222a01        21 minutes ago      188MB
```

この Docker イメージで Dockerfile に足していくのかな？

では、試行錯誤する用コンテナを立てるか。
```shell
    $ docker run -it -p8000:80 --name web02 tada/httpd:ver1.1

    # apt install -y emacs
    # apt install -y iproute2
```
うまくいくので、これを Dockerfile に新たにインテグレートしてみる。

```shell
FROM tada/httpd:ver1.1
MAINTAINER George Tada

RUN apt update && apt install -y apache2
ADD index.html /var/www/html/index.html

RUN apt install -y emacs
RUN apt install -y iproute2  # ip コマンドが使えるようになる

CMD apachectl start && bash
```

Docker イメージを作る。
```shell
    $ docker build -t tada/httpd:ver1.2 .
```
コンテナを立てる。

```shell
    $ docker run -itd -p8000:80 web03 tada/httpd:ver1.2
```

Dockerfile に CMD を入れてないけどいいみたい。
だけど、追加していくのがいいかな。
前のがプラックボックス化しないこと。


