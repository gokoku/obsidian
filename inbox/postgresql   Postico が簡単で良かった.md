---
type: note
---

#postgresql 

---
2022-10-11  15:54

# postgresql   Postico が簡単で良かった

https://eggerapps.at/postico/

![[Pasted image 20221011161750.png]]

https://qiita.com/junara/items/8ee572151727288f6576

![[Pasted image 20221011161837.png]]

POP OS で Docker 上で PostgreSQL を立ち上げてる。

localhost で
user : postgres
pass : postgres

で接続した。



クライアントマシンで SSH ポートフォワーディングしてる前提。

```shell
$ ssh -NL 127.0.0.1:5432:127.0.0.1:5432 pop
```

pop は .ssh/config で設定している。
```config
Host pop
  Hostname pop-os.local
  User george
```


![[Pasted image 20221011162049.png]]

わかりやすい。

![[Pasted image 20221011162110.png]]

これが欲しかったのよ。





