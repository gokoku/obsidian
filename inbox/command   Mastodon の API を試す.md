#command

---
2021-11-12

# Mastodon API で Delete できるか

Android 本で Delete が上手くいかないので、手動で出来るかやってみた。
Android で id は見れる。Token は https://pawoo.net/ の設定で見れる。

API のドキュメントはここ。

https://docs.joinmastodon.org/methods/statuses/


id はURL に、ヘッダには Authorization で Bearer <user access token> を噛ませればいいようだ。
	
```shell
$  curl -X DELETE -H "Authorization:Bearer FKH1ufCPFPgcNtI9KOFHNXoluta9LmOOgrHf_jq_Q8I"  https://pawoo.net/api/v1/statuses/107262408013646178
```

出来た!!

AccessToken の前に Bearer を入れるのを忘れないように。
	
てゆーか、Bearer の綴り間違えてた...