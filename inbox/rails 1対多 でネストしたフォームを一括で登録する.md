---
type: note
---

#rails

---
2022-11-10  13:56

# rails 1対多 でネストしたフォームを一括で登録する



<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/kyokucho1989/items/a1d971bbc6e15888a713" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9MSVFNSVBRiVCRSVFNSVBNCU5QSVFMyU4MSVBRSVFOCVBNiU4MSVFNyVCNCVBMCVFMyU4MiU5MjElRTMlODElQTQlRTMlODElQUV2aWV3JUUzJTgxJUE3JUU3JTk5JUJCJUU5JThDJUIyJUUzJTgxJTk5JUUzJTgyJThCJTJGJUU1JTgwJThCJUU0JUJBJUJBJUU5JTk2JThCJUU3JTk5JUJBJUUzJTgxJUFFJUUzJTgxJUE0JUUzJTgxJUE1JUUzJTgxJThEJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz01NjY4ZWI4MzlhYzc3MjBhMmE5NGZmZWYyYzYwNGMwOQ&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwa3lva3VjaG8xOTg5JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz1lNjBmNTZlYTJiYzQ3ZGQxMjJlM2RiYjA0ODdhM2FlMA&blend-x=142&blend-y=491&blend-mode=normal&s=8def0b0ac6b75010d9e7fb2a202dbdab')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">1対多の要素を1つのviewで登録する/個人開発のつづき - Qiita</h1>
		<p class="rich-link-card-description">
		はじめに ずっとコードを書いていたが、それだけではだいじなことを忘れそうな気がしたので、ここに記録しておこう。今、個人開発で日報アプリをつくろうとしている。ユーザーは日々の記録を書ける。各作業のジャンル、作業名、何時間したかさ...
		</p>
		<p class="rich-link-href">
		https://qiita.com/kyokucho1989/items/a1d971bbc6e15888a713
		</p>
	</div>
</a></div>



今回参考になったのはこの記事↓

<div class="rich-link-card-container"><a class="rich-link-card" href="https://railsguides.jp/form_helpers.html#%E8%A4%87%E9%9B%91%E3%81%AA%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://railsguides.jp/images/cover_for_facebook.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Action View フォームヘルパー - Railsガイド</h1>
		<p class="rich-link-card-description">
		ビルトインのフォームヘルパーについて解説します。
		</p>
		<p class="rich-link-href">
		https://railsguides.jp/form_helpers.html#%E8%A4%87%E9%9B%91%E3%81%AA%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B
		</p>
	</div>
</a></div>

ネストしているとは、1対多リレーションでのフォームの作りのこと。


![[Pasted image 20221110140705.png]]

```ruby
class DesignedDocument < ApplicationRecord
  has_many :dd_texts, dependent: destroy
  accepts_nested_attributes_for :dd_texts, allow_destroy: true
```


```ruby
class DdText < ApplicationRecord
  belongs_to :designed_doument
end
```


ネストしたフォームを作る。
複数個 DdText がぶら下がる。

```ruby
<%= form_with(model: designed_document, id: 'document_form') do |form| %>
  ...
  <%= form.field_for :dd_texts do |dd_text_form| %>
	<%= dd_text_form.text_area :body %>
  <% end %>
  ...
<% end %>
```

![[Pasted image 20221110141329.png]]

これで、update で投げられるパラメータは、

```ruby
Parameters: {
  "authenticity_token"=>"[FILTERED]",
  "designed_document"=>{
	...
    "dd_texts_attributes"=>{
		"0"=>{"body"=>"人口あう禍根。きゅうりょう牛乳お盆。電話隆起問題。\r\n", "id"=>"79"},
		"1"=>{"body"=>"こわすちかくこうおつ。ハチのすむく普及。復旧ぼきん運ぶ。", "id"=>"80"},
		"2"=>{"body"=>"あいうえお", "id"=>"81"}
	},
	"status"=>"published",
	"published_at"=>"2022-08-24T23:24:01"},
	"commit"=>"更新",
	"id"=>"47"
}
```

パラメータがネストしているのがわかる。

これをコントローラで通るように許可を記述する。

```ruby
    def designed_document_params
      params.require(:designed_document).permit(
		...
        :status,
        :published_at,
		...
        dd_texts_attributes: [:id, :body]
      )
    end
```

これでぶら下がっている DdText の中身も全部更新してくれる。

