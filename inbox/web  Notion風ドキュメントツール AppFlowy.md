#web 

---
2021-11-18

# AppFlowy 

notion風ドキュメントツール

Flutter Rust

https://www.appflowy.io/


https://github.com/AppFlowy-IO/appflowy

## Getting Started

rust の cargo がないと失敗する。

[[rust  Rust インストール]]

cargo-make も必要だったようだ。

```shell
$ cargo install --force cargo-make
```


```shell
$ git clone https://github.com/AppFlowy-IO/appflowy.git
$ cd appflowy
$ make install_rust
$ make install_cargo_make
$ cargo make install_targets
```

vscode を開いて flutter を動かすらしいが、上手くいかなかった。
