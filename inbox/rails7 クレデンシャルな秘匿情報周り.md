---
type: note
---

#rails

---
2023-04-24  18:14

# クレデンシャルな秘匿情報周り

config/credentials.yml.enc に書くのがいいみたいだ。

master.key で暗号を掛けて credentials.yml.enc に書き込むらしい。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://techtechmedia.com/credentials-masterkey-rails/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://techtechmedia.com/wp-content/uploads/2020/07/cropped-favicon-192x192.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">【Rails5.2】秘匿情報を管理することができるcredentials.yml.encとmaster.keyの使い方について｜TechTechMedia</h1>
		<p class="rich-link-card-description">
		今回はRails5.2で追加された秘匿情報を管理する仕組み、credentials.yml.encとmaster.keyの使い方について詳しく解説しています。AWSキーやAPIキーといった秘匿情報を管理する仕組みはRails5.2以前からもありましたが、Rails5.2からはcredentials.yml.encでの管理に変更になりました。
		</p>
		<p class="rich-link-href">
		https://techtechmedia.com/credentials-masterkey-rails/
		</p>
	</div>
</a></div>


この credentials.yml.enc を暗号解除して開くには
```shell
$ EDITOR='emacs' rails credentials:edit
```

```
# aws:
#   access_key_id: 123
#   secret_access_key: 345

# Used as the base secret for all MessageVerifiers in Rails, including the one protecting cookies.
secret_key_base:87a29ef32eab41b3ee9d20ace29e35cc9f90dfccaef3e06e3d4747e9135efbd95bbaa13c6b4ec50eeb\
 f20e6b903f06c48e76a1dbf509fa7f39750cd266adc81f

# ここにデータベースのパスワードを追記する
gtfs_database_password: pmorioka7481
```

データベースの production のところをこのコマンドに書き換える
```shell
$ rails c

> Rails.application.credentials[:gtfs_database_password]
 "pmorioka7481"
```

config/database.yml

```yaml

...
production:
  <<: *default
  database: gtfs_production
  username: gtfsuser
  password: <%= Rails.application.credentials[:gtfs_database_password] %>
```



これはいい。
master.key がなければこれは解読出来ないセキュアな今の rails の答えか。

