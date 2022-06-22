#raspberry_pi 

---
2022-03-14  13:27

# raspberry_pi  Tmux の設定

tmux の設定はこれが便利。

~/.tmux.conf

```
set -g mouse on
bind-key -T copy-mode Enter send-keys -X copy-pipe-and-cancel "xsel -i p && xsel -o -p | xsel -i -b"
```

xsel でクリップボードが使えるようになるには、
```shell
$ sudo apt install xsel
```

set -g mouse on がかなり便利。

これがあると、マウスで自動でコピーモードになるので、スクロールしてくれるようになる。

![[command  Tmux のコピーモードを使いこなしたい]]