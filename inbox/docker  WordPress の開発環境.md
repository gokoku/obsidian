#docker #wordpress


https://goworkship.com/magazine/wordpress-docker/

docker-compose.yml
```docker
version: '2' #　Composefileのバージョンを2に設定

services:
  wordpress:
    image: wordpress:latest # MySQL5.7公式イメージを利用
    ports:
      - "3001:80" # ポート番号の設定
    depends_on:
      - mysql # mysqlを立ち上げた後にWordpressを立ち上げる
    env_file: .env # 環境変数の定義に.envを利用
    volumes:
        - ./wp-content:/var/www/html/wp-content # マウントするディレクトリを指定

  mysql:
    image: mysql:5.7 # MySQL5.7公式イメージを利用
    env_file: .env # 環境変数の定義に.envを利用
    ports:
      - "3306:3306" #ポート番号の設定
```

.env
```env
WORDPRESS_DB_NAME=wordpress
WORDPRESS_DB_USER=wp_user
WORDPRESS_DB_PASSWORD=hogehoge

MYSQL_RANDOM_ROOT_PASSWORD=yes
MYSQL_DATABASE=wordpress
MYSQL_USER=wp_user
MYSQL_PASSWORD=hogehoge
```

```shell
$ mkdir wp-content

$ docker-compose up -d
OK
```



```shell
$ docker-compose down
```