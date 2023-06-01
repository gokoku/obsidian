---
type: note
---

#blender 

---
2022-09-20  13:48

# blender. Add-ons で stable diffuser を使うやつ Dream Textures

https://twitter.com/Yamkaz/status/1571449147127103489

ここからDL

https://github.com/carson-katri/dream-textures/releases/tag/0.0.5

![[Pasted image 20220920134952.png]]

rust をinstall
```shell
$ asdf plugin-add rust
$ asdf install rust latest
$ asdf reshim rust
$ asdf list rust
$ asdf global rust 1.63.0
$ cargo --version
cargo 1.63.0
```

install Dependencies で Apple Silicon しかない。POP-OS でやろっか。
chromebook POP-OS は Dependencies で失敗する。

