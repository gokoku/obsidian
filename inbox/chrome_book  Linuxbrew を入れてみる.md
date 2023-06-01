#chrome_book


Linux brew を入れる。

```shell
$ sudo apt update
$ sudo apt install build-essential curl file git

なぜか失敗して
$ sudo apt update
$ sudo apt install build-essential curl file git procps

OK
```

### ユーザーにパスワードを設定するのが最初がいいかも

```jsx
$ sudo passwd george

cpu68060
```


https://docs.brew.sh/Homebrew-on-Linux

```shell
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"



$ test -d ~/.linuxbrew && eval $(./linuxbrew/bin/brew shellenv)
$ test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
$ test -r ~/.bash_profile && echo "eval \$($(brew --prefix)/bin/brew shellenv)" >> ~/.bash_profile
$ echo "eval \$($(brew --prefix)/bin/brew shellenv)" >> ~/.profile
```

```shell
$ brew install gcc        <----recomend というので
$ brew install emacs

```
