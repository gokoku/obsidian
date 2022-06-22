#raspberry_pi 

---
2022-03-10  11:40

# raspberry_pi で asdf-vm を導入する

```shell
$ git clone https://github.com/asdf-vm/asdf.git ~/.asdf
$ cd ~/.asdf
$ git checkout "$(git describe --abbrev=0 --tags)"
```

~/.bashrc

```shell
. $HOME/.asdf/asdf.sh
. $HOME/.asdf/completions/asdf.bash
```

