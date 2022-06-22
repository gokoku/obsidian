#github

---
2022-03-22  10:51

# github   接続できなくなった

今日繋がらなくなった。push も pull も clone も出来ない。

接続テスト
```shell
$ ssh -T git@github.com
```

このままでは接続しない。
~/.ssh/config に 設定してやる。
```config
Host *
  IPQos 0
```
cs1 以外でも 0x00 でも none でも良いようだ。

これは DSCP というパケットデータグラムの値で、フレッツ網が drop してしまう現象らしい。
そして、SSH からこの値を設定するのがこの IPQos オプションのようだ。

フレッツ回線の仕様で、IPv6 パケットの DSCP 値によって drop されていた。

つまり、会社のネットを使ってるからこの現象に出会したのか。
家では大丈夫なわけだ。

```shell
$ ssh -T git@hithub.com
Enter passphrase for key '/Users/george/.ssh/id_rsa':
Hi gokoku! You've successfully authenticated, but GitHub does not provide shell access.
```

接続成功。

この状態で git を使ってみる。

出来た。