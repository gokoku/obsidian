---
type: note
---

#pop_os 

---
2022-05-08  21:17

# pop_os  Zsh を設置した

Github から zsh を clone する。

~/.zshenv
```shell
export ZDOTDIR=$HOME/zsh
```

fzf は linuxbrew で入れた。

```shell
$ brew install fzf
```

```shell
$ /home/linuxbrew/.linuxbrew/Cellar/fzf/0.30.0/install

$ /usr/local/Cellar/fzf/0.30.0/install

Generate ~/.fzf.bash
Generate ~/.fzf.zsh
```



```shell
# Setup fzf
# ---------
if [[ ! "$PATH" == */Users/george/.fzf/bin* ]]; then
  export PATH="${PATH:+${PATH}:}/Users/george/.fzf/bin"
fi

# Auto-completion
# ---------------
[[ $- == *i* ]] && source "/Users/george/.fzf/shell/completion.zsh" 2> /dev/null

# Key bindings
# ------------
source "/Users/george/.fzf/shell/key-bindings.zsh"
```


```shell
$ sudo apt install zsh
$ sudo apt install zsh-autosuggestions
$ sudo apt install ripgrep

$ git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k

$ sudo mv ~/powerlevel10k /usr/local/opt
$ sudo ln -s /usr/share/zsh-autosuggestions /usr/local/share/
```

```shell
$ sudo apt install zsh
$ sudo apt install zsh-autosuggestions
$ sudo apt install ripgrep

$ git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k

$ sudo mv ~/powerlevel10k /usr/local/opt
$ sudo ln -s /usr/share/zsh-autosuggestions /usr/local/share/
```


docker が入ってること。
これ Docker でやらないと、Python 環境が刺さりまくってコケる。

```shell
# Nerd font install
$ git clone --depth=1 https://github.com/romkatv/nerd-fonts.git
$ cd nerd-fonts
$ sudo ./build 'Meslo/S/*'
$ ./install.sh SourceCodePro
```


### これMac でやってみたら build 途中で失敗するが

nerd-fonts/patched-fonts/Meslo/ の中のcomplete の中の ttf が出来てる。

![[Pasted image 20220522201742.png]]

これを全部さらって Font Book に放り込む。

![[Pasted image 20220522201835.png | 400]]

https://github.com/ryanoasis/nerd-fonts

![[Pasted image 20220522202041.png | 500]]

てゆーか、Docker は Linux のものだから、POP!_OS から TTF ファイルをもらおうか。
家の Mac ではもう Docker for mac が調子悪すぎて動かない。

---

再起動。

ターミナルの設定で、SaurceCodePro Nerd Font が見つかるので、それに設定する。

シェルを zsh にする。

```shell
$ chsh

/usr/bin/zsh
```



