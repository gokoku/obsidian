---
type: note
---

#pop_os #asdf #chrome_book 

---
2022-05-09  11:46

# pop_os   ASDF-VM で ruby 他を入れる

```shell
$ asdf plugin add ruby
$ asdf install ruby 3.1.2

まず失敗
```

```shell
$ sudo apt install -y libssl-dev

$ asdf install ruby 3.1.2
成功!!
```

なんとこれだけでいった。


