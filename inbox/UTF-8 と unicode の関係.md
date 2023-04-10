---
type: note
---

#command

---
2022-07-04  10:39

# UTF-8 と unicode の関係


<div class="rich-link-card-container"><a class="rich-link-card" href="http://equj65.net/tech/charcode/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('http://equj65.net/wp-content/uploads/2014/03/characters.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">文字コードの考え方から理解するUnicodeとUTF-8の違い</h1>
		<p class="rich-link-card-description">
		UnicodeとUTF-8の違いを文字コードの考え方から解説。また、UnicodeとUTF-8を混合しやすい理由を考える。
		</p>
		<p class="rich-link-href">
		http://equj65.net/tech/charcode/
		</p>
	</div>
</a></div>


Unicode と UTF-8 は違う。

![unicodeimage-1](http://equj65.net/wp-content/uploads/2014/03/unicodeimage-1.jpg)

Unicode の値の集まり --> 符号空間 codespace

符号空間の個々の値の位置 --> 符号位置 code points

符号位置の表現 `U+3042`  あ

![[スクリーンショット 2022-07-04 10.47.50.png]]


ということで、unicode は文字のマッピングテーブルで仮想的な符号空間。

```ruby
$ irb
> "あ".ord.to_s(16)
"3042"

> "\u3042"
 "あ"
```

コードポイントでデコードすれば Encoding が UTF8 であれば、そのまま文字の値となるのか。

### UTF-8

文字コードに当たるのかな。

```ruby
> "あ".unpack("H*")
["e38182"]


> "\xe3\x81\x82"
"あ"
```



