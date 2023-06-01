---
type: note
---

#mint

---
2022-05-05  20:45

# mint Linuxbrew を入れる

[[mint Linuxbrew を入れる]]

```shell
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"



$ test -d ~/.linuxbrew && eval $(./linuxbrew/bin/brew shellenv)
$ test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
$ test -r ~/.bash_profile && echo "eval \$($(brew --prefix)/bin/brew shellenv)" >> ~/.bash_profile
$ echo "eval \$($(brew --prefix)/bin/brew shellenv)" >> ~/.profile
```

この .bash_profile に書くものは、その前の eval で brew が通ってるから、

```shell
eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
```

を .zshrc に書く。

