#command 

---
2022-01-24  14:32

# command  SSH でパスフレーズを聞かれないようにするには

SSH で公開鍵認証してても、秘密鍵のパスフレーズを聞かれる。

key-agent に活躍してもらいたいのだが、ログインするたびに空っぽになる。

key-add で登録すると、なんかワーニングが出る。keychain があるよ的な。



.ssh/config でこんな設定するとうまく出来るみたいだ。

```shell:.ssh/config
Host *
  ForwardAgent yes
  UseKeychain yes
  AddKeysToAgent yes
  Identityfile ~/.ssh/id_rsa
```

これで GitHub に Push Pull しても、パスフレーズを聞かれなくなった。

ログインした時点では、key-agent に登録されてない。
```shell
$ key-add -l
The agent has no identities.
```

でも、ssh を使うと、この設定が動いて自動的に登録されるようだ。
Github に git push してみた後でそれが確認出来た。

```shell
$ key-add -l
4096 SHA256:t/XALLxKO9WyzGcYXQhDgEjk9upza+nCtOh7i3pjV3Y george.6809@gmail.com (RSA)
```

これで行こう。

