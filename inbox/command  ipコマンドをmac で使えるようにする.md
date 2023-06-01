---
type: note
---

#command #osx 

---
2022-12-20  14:38

# command  ipコマンドをmac で使えるようにする

```shell
$ brew install iproute2mac

$ ip a

lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
	inet 127.0.0.1/8 lo0
	inet6 ::1/128
	inet6 fe80::1/64 scopeid 0x1
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	ether 78:7b:8a:d5:76:90
	inet6 fe80::862:dab:ffa6:2a03/64 secured scopeid 0x4
	inet6 2400:4053:4540:3700:1822:8a5c:399d:f89b/64 autoconf secured
	inet6 2400:4053:4540:3700:ad9d:a642:83a6:3ca7/64 autoconf temporary
	inet 192.168.20.50/24 brd 192.168.20.255 en0
....

```

