---
type: note
---

#osx

---
2022-12-20  13:15

# osx Ventura で Eather ネットワークが設定出来ない

環境設定でネットワークで Eather が設定出来ない。

```shell
$ networksetup -listallhardwareports
Hardware Port: Ethernet
Device: en0
Ethernet Address: 78:7b:8a:d5:76:90
...
```

wireshark で en0 を見ても、データが流れてるのが見える。

```shell
$ networksetup -getcomputername
iMac21-2017-01
```

どこで設定してあるのか?
共有でローカルホスト名が設定されている。

# よくわからんが治った

thundervolt ブリッジとか仮想インタフェースを全部削除して、再起動したら Ethernet が復活してた。
自動で作られてた。

