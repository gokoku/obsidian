#command 

---
2022-03-08  10:32

# command  Dnsmasq を使う

```shell
$ brew install dnsmasq
```

サーバの /etc/hosts に記述。

```shell
$ sudo brew services start dnsmasq
$ sudo brew services restart dnsmasq
$ sudo brew services stop dnsmasq
```

sudo 付けないと、53ポートでの Listen が設定出来ないためらしい。

.local は Bonjour で使うので、使用しないこととのこと。

