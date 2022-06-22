---
type: note
---

#mysql

---
2022-05-06  11:47

# mysql  Sequel Ace で HTTP Proxy コマンドを使えるようにしたい

Corkscrew で失敗してたので、connect にしてみた。

色々、試したが、connect でやっと上手く ssh してくれた。

が、connect が Permission error で動かなくて、Sequel Ace は ssh でコケてる。

実は、corkscrew でも connect でも Ace の中では動かないようだ。

sandbox の中で動くので、どうしても外部は permition error になる?

# Ace の中でなく、外で動かせばいい

ssh のポートフォワーディング機能で MySQL を仲立ちさせる。

ssh 自体は HTTP Proxy で接続するので、connect なり corkscrew で接続させる。

### TCP FORWARDING on backgound

![[ssh 2022-05-02 17.59.30.excalidraw | 600]]

.ssh/config では taki で HTTP proxy を介してサーバに接続出来るようにしている。

```
Host taki
  Proxycommand env CONNECT_USER=people CONNECT_PASSWORD=p30pl312E /usr/local/bin/connect -H ns2.people.co.jp:8080 %h %p
  HostName takizawanavi.jp
  User takizawausr

```

```jsx
$ ssh -f -N -L 3306:127.0.0.1:3306 -o ExitOnForwardFailure=yes taki 
```

-f でバックグラウンド処理、ExitOnForwardFilure=yes でパスフレーズを入力が forground に出て入力出来るようになる。

3306:127.0.0.1:3306 は、ローカル側 : サーバ側で、ローカル側は 127.0.0.1 がデフォルトで省略されてるようだ。

なので、127.0.0.1 の 3306ポートに接続するとサーバの MySQL に直に接続出来る。
```shell
$ mysql -h 127.0.0.1 -u takizawanavidb -p 
... 
mysql>
```

-f でバックグラウンドにすると、process がこれになってる。

```shell
/usr/local/bin/connect -H ns2.people.co.jp:8080 takizawanavi.jp 22
```

### TCP FORWARDING on forground

```shell
$ ssh -NL 3306:127.0.0.1:3306 taki
```

ctrl - c で止められるので扱い易いかな。


### MySQL client 接続

これで、Sequel Ace だろうが、なんだろうが、単純にDB接続出来る。

![[Pasted image 20220506115304.png]]
![[Pasted image 20220506115327.png]]

