---
type: note
---

#raspberry_pi 

---
2022-12-13  17:31

# raspberry_pi  別ネットワークに設定した pi を確認したい

風の丘のラズパイを固定 IP にした。windows が abahi に対して名前解決が不安定だった対策。

盛岡事業所でこのネットワーク設定をするとどこからも見れなくなる。

# Mac に仮想インターフェースを設定する

![[Pasted image 20221213173510.png | 600]]

環境設定からネットワーク、下の点々から仮想インターフェースを管理からブリッジ追加する。

![[Pasted image 20221213173649.png|500]]
インターフェースを Ethernet を選択する。
![[Pasted image 20221213173742.png]]
ブリッジの設定をする。
![[Pasted image 20221213173856.png]]

この時点で、ラズパイが見えた。

![[Pasted image 20221213174021.png]]

ssh で接続出来る。

```shell
$ ssh pi
> cat /etc/dhcpcd.conf


interface wlan0
inform 192.168.165.252
static routers=192.168.165.254
static domain_name_servers=192.168.165.254
static domain_search=192.168.165.254
```

ラズパイのネットワーク設定が確認出来た。

# 実はこんなことしなくても ssh 接続できる

ネットワーク名で接続すると V6 で解決するみたいだ。

```shell
$ ssh pi@people.local
>
```





