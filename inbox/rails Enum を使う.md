---
type: note
---

#ruby/rails 

---
2022-11-08  16:28

# rails Enum を使う

Model の中で、定義する。

```ruby
class DesignedDocument < ApplicationRecord

  enum status: {
	  draft: 0,
	  publiched: 1,
	  private:  2
  }, _prefix: true_
```

`_prefix` は名前空間的に、重複しないように接頭辞につけてアクセスできるようにするものとのこと。
必ずつけた方がいいらしい。
7 からは アンダースコアは入らなくなったとのこと。
```ruby
document.status_draft
```


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/emacs_hhkb/items/fce19f443e5770ad2e13#_prefix%E6%8E%A5%E9%A0%AD%E8%BE%9E_suffix%E6%8E%A5%E5%B0%BE%E8%BE%9E%E3%82%92%E4%BD%BF%E3%81%86" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9UmFpbHM1JTIwJUUzJTgxJThCJUUzJTgyJTg5JTIwZW51bSUyMCVFNCVCRCVCRiVFMyU4MSU4NiVFNiU5OSU4MiVFMyU4MSVBRl9wcmVmaXglRUYlQkMlODglRTYlOEUlQTUlRTklQTAlQUQlRTglQkUlOUUlRUYlQkMlODlfc3VmZml4JUVGJUJDJTg4JUU2JThFJUE1JUU1JUIwJUJFJUU4JUJFJTlFJUVGJUJDJTg5JUUzJTgyJTkyJUU0JUJEJUJGJUUzJTgxJThBJUUzJTgxJTg2JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz1jOWI5ZTc3NWMxYTgyZWE1ZWE3MWQ5MTY4YjExM2ZiYg&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwZW1hY3NfaGhrYiZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9MjMxYjgxY2MzOWE1M2JmMWEwYzM3NmMyZTFkNGYxNGI&blend-x=142&blend-y=491&blend-mode=normal&s=f34ed71d4a25ad76b9599b27ac0224d9')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Rails5 から enum 使う時は_prefix（接頭辞）_suffix（接尾辞）を使おう - Qiita</h1>
		<p class="rich-link-card-description">
		ニュース Rails7 がリリースされたことで enum 構文の記載方法が少し拡張されました。（得られる結果は現状同じ） - Before（Rails5, Rails6）  Key-Valueとして列挙していた enum sta...
		</p>
		<p class="rich-link-href">
		https://qiita.com/emacs_hhkb/items/fce19f443e5770ad2e13#_prefix%E6%8E%A5%E9%A0%AD%E8%BE%9E_suffix%E6%8E%A5%E5%B0%BE%E8%BE%9E%E3%82%92%E4%BD%BF%E3%81%86
		</p>
	</div>
</a></div>

## Locale を使う


<div class="rich-link-card-container"><a class="rich-link-card" href="https://sonnaka.com/rails-enum-conversion/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://sonnaka.com/wp-content/uploads/2021/01/text.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Railsでenumを自由自在に操る</h1>
		<p class="rich-link-card-description">
		enumは、DBには数字で保存でき、DRUDは人間の言葉で操作できる、大変便利な仕組みですよね。 しかし、数字／英語／日本語の３つの表現があるために思うようにいかないときもあるのでは？ 初心者にもわかるよう、それぞれの対応を整理しておきます。
		</p>
		<p class="rich-link-href">
		https://sonnaka.com/rails-enum-conversion/
		</p>
	</div>
</a></div>

gem が必要らしい。

```ruby
# _i18n をつけて定数を翻訳
gem 'enum_help'
```


config/application.rb で locales を使えるようにする。

```ruby
module Ogasta
  class Application < Rails::Application
    config.i18n.default_locale = :ja
    # I18n ライブラリに訳文の探索場所を追加する
    config.i18n.load_path += Dir[Rails.root.join('config', 'locales', '**', '*.{rb,yml}').to_s]
```

モデル毎にja.yml を設定しておくと分離がいいね

config/locales/models/designed_document.ja.yml

```yaml
ja:
  activerecord:
    models:
      designed_document: ドキュメント
      ...
  enums:
    designed_document:
      status:
        draft: 下書き
        published: 公開
        private: 非公開
```

こうしとくと、このように使えるようになる。

```ruby
document.status_i18n   #値が日本語で取得出来る。



DesignedDocument.statuses_i18n　
=> {"draft"=>"下書き", "published"=>"公開", "private"=>"非公開"}
```

