#js

---
2021-07-13


# Next.js を自前でデプロイするには?

next.js を Apache で公開するには、リバースプロキシを利用するらしい。

https://suwaru.tokyo/%E3%80%90%E6%9C%AC%E7%95%AA%E3%83%AA%E3%83%AA%E3%83%BC%E3%82%B9%E3%80%91node-js%E3%82%B5%E3%83%BC%E3%83%90%E6%8E%A5%E7%B6%9A%E6%96%B9%E6%B3%95%E3%80%90%E3%83%AA%E3%83%90%E3%83%BC%E3%82%B9%E3%83%97/

localhost:3000 にアクセスするようにして、ドキュメントルートを設定するとな。

いくつも立てられんのかな。

```config
DocumnetRoot "/var/www/html/app-path"

<Directory "/var/www/html/app-path">



ProxyRequests Off

<Location>
   ProxyPass http://localhost:3000/
   ProxyPassReverse http://localhost:3000/
</Location>
```

```shell
$ sudo service httpd restart
```

Node.js サーバを起動する。
```shell
$ nohup yarn start &
```

nohup :   SIGHUPシグナルを受け付けなくなって終了しなくなるとのこと。

