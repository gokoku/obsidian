
---
type: note
---

#ruby/rails 

---
2022-11-08  18:10

# rails 7 で アイコンフォントを使う


<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.youtube.com/watch?v=c-EbQDB0RsQ" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.youtube.com/embed/c-EbQDB0RsQ?feature=oembed')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Ruby on Rails #76 Fontawesome with Importmaps in Rails 7</h1>
		<p class="rich-link-card-description">
		Episode source code: https://github.com/corsego/76-fontawesome-rails7-importmaps/commit/669b9a5b1a0a064d55c1d5ec6b5fa95e450fd005Text guide: https://blog.cors...
		</p>
		<p class="rich-link-href">
		https://www.youtube.com/watch?v=c-EbQDB0RsQ
		</p>
	</div>
</a></div>


https://qiita.com/gazayas/items/9224b22ce6416a624f33

```shell
$ yarn add @fortawesome/fontawesome-free

$ bin/importmap pin @fortawesome/fontawesome-free
```

config に importmap.rb ができてるので、編集する。file name --> all.js
```ruby
pin "@fortawesome/fontawesome-free", to: "https://ga.jspm.io/npm:@fortawesome/fontawesome-free@6.2.0/js/all.js"

```

app/javascript/application.js に追加
```js
import "@fortawesome/fontawesome-free";
```

index.html.erb

```html
<i class="fa-solid fa-pen-to-square"></i>

<i class="fa-solid fa-trash"></i>

<i class="fa-solid fa-clone"></i>
```


![[Pasted image 20221108182859.png]]

出た。

# icon の探し方

https://fontawesome.com/search

![[Pasted image 20221108183009.png]]

ここで、Free を選択しとけば間違いないけど、全開でもいいみたい。

### カスタマイズ

https://fontawesome.com/docs/web/style/styling

color

```html
<i class="fa-solid fa-address-book" style="color: green"></i>
```


size

```html
<i class="fa-solid fa-coffee fa-2xs"></i>
```

| Relative Sizing Class | Font Size | Equivalent in Pixels |
| --------------------- | --------- | -------------------- |
| fa-2xs                | 0.625em   | 10px                 |
| fa-xs                 | 0.75em    | 12px                 |
| fa-sm                 | 0.875em   | 14px                 |
| fa-lg                 | 1.25em    | 20px                 |
| fa-xl                 | 1.5em     | 24px                 |
| fa-2xl                | 2em       | 32px                 |





