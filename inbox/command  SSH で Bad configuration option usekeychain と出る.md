#commnad

---
2022-03-22  09:57

# command  SSH で Bad configuration option usekeychain と出る

openssh を入れたら接続時に出るようになった。

オプションが無くなったのか?

とりあえず、無視してくれと一言付け加えると、全部削除しなくて済む。

```config
Host *

	IgnoreUnknown UseKeychain
	

```

いや面倒なので、Usekeychain を全部削除しよう。
