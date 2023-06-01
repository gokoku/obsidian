#php

---
2021-12-16

# レスポンスヘッダの Content-Type の指定

php.ini
```
default_charset = "UTF-8"
```

```shell

$ service httpd restart

```

php の中で指定する場合は

```php

header("Content-type: text/html; charset=utf-8");

```

とやるとのこと。
これかな。


	