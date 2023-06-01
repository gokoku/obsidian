---
type: note
---

#command

---
2023-04-10  11:57

# ssh  no matching host key type found. で入れない

https://note.com/niwaken/n/n7caf20e585ef


.ssh/config
```
Host sylvanian
  Hostname 210.129.52.39
  User people
  HostKeyAlgorithms ssh-dss,ssh-rsa
  PubkeyAcceptedAlgorithms +ssh-rsa
```

下2行を加えると繋がる。

https://qiita.com/narikei/items/363f9a61a7faf6c3480f

OpenSSH が新しいと、古いサーバに入るとこうなるらしい。
脆弱性のある暗号化方式に対応しないぜ、みたいなエラーとのこと。

SHA-1 ハッシュアルゴリズムのRSA署名は無効化
