#command

---
2022-03-09  10:09

# command  Tmux のコピーモードを使いこなしたい

emacs バインドか、viバインドか。

mac のデフォルトは vi だったが、ssh で raspberry pi に入ると vi では動かない。

設定が必要なのだろうか。

### raspberry pi は設定が必要

xsel でクリップボードが使えるようにする準備。

```shell
$ sudo apt install xsel
```

~/.tmux.conf

```shell:~/.tmux.conf
bind-key -T copy-mode Enter send-keys -X copy-pipe-and-cancel "xsel -i p && xsel -o -p | xsel -i -b"
```

```
C-b [ コピーモードに入る。

カーソル移動
Ctrl + space でコピー開始
Enter でコピーモード終了

C-b ] でペースト
```

やっぱ Emacs バインドがスムーズでいいな。

### mac も Emacs バインドにする
```shell:~/tmux.conf
set -g mode-keys emacs
set -g mouse on
bind-key -T copy-mode Enter send-keys -X copy-pipe-and-cancel "pbcopy"
```

これで、同じ動作になった。

