#chrome_book 

# Linux コンテナに外からアクセスするには

### Linux環境にポートフォワード

設定→Linux→ポート転送

ポートを追加

2000 (TCP)    sshd

### コンテナの IP アドレス

```jsx
$ ip a
100.115.92.202/28
```

### Chromebookの IPアドレス

設定→ネットワーク→Wifi

今日のip 192.168.20.120

### SSHD を立てる

openssh-server はもう入っていたが、error で active にならない。

```jsx
$ sudo systemctl status ssh.service
```

sshd_not_to_be_run が邪魔らしい。__sshd_not_to_be_run にリネームしてスタート。

```jsx
$ sudo systemctl start ssh.service
```

起動することがわかった。

### ポートをフォワーディングしたものに設定する。

```jsx
# /etc/ssh/sshd_config

Port 2000
AddressFamily any            <---ここからはコメントインしただけ
ListenAddress 0.0.0.0
ListenAddress ::
```

### ユーザーにパスワードを設定

このままだとパスワードにはじかれる。

どうも設定してないだけらしい。  ユーザー名は george

```jsx
$ sudo passwd george

```

## 外のマシーンから入る

```jsx
$ ssh geoge@192.168.20.120 -p 2000
password: xxxxxx

> 入った!!!
```
