---
type: note
---

#ruby #ruby/rails 

---
2022-12-23  13:56

# ruby 画像サイズを取得する gem

https://hivecolor.com/id/147

rails では Gemfile に入れるだけで、特にrequireしなくても使える

```ruby
gem 'fastimage'
```


cropperjs でオブジェクトを作るときに元画像の大きさを指定しないと、デフォルトの200x100 になるので、予め取得して指定する必要があった。

photo .file は carryWave のファイルらしく、path も難なく取得できる優れもの。

```html
<% photo_size = FastImage.size( photo.file.path ) %>
<%# 画像のサイズを取得する [x, y] %>

<script type="module">
   const cropper_<%= photo.id %> = new Cropper(cropperImg_<%= photo.id %>, {
   
	   minContainerWidth: <%= photo_size[0] %>,
	   minContainerHeight: <%= photo_size[1] %>,
</script>   
   
   })

```


