---
type: note
---

#js #ruby

---
2022-06-17  18:10

# js RSS を読ませる方法

Ajax で RSS を読むことは CROS に引っかかってだめ。

curl かなんかで rss をダウンロードしておいて、それを読み込むようにすると上手くいく。

```shell
$ curl -OL https://www.pref.iwate.jp/agri/i-agri/feed.rss
```

sinatra で簡易サーバを立てる。

```shell
$ gem install sinatra
$ gem install sinatra-reloader
```
myapp.rb

```ruby
require 'sinatra'
require 'sinatra/reloader'

get '/' do
  File.read('index.html')
end

get '/rss' do
  File.read('feed.rss')
end
```

```shell
$ ruby myapp.rb
```

ここは Apache になるところ。

###  jQuery を使うと xml が find でアクセス出来る!!

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
    <script
      src="https://code.jquery.com/jquery-3.6.0.min.js"
      integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4="
      crossorigin="anonymous"></script>
  </head>
  <body>
    <div id="rss"></div>
    <script>
      $.ajax({
        type: "get",
        url: "rss"
      }).done(function(result) {
        $(result).find("item").each(function() {
          console.log($(this).find("title").text());
          $("#rss").append(
            "<div class='rss-item'><a href='" +
              $(this)
                .find("link")
                .text() +
              "'>" +
              $(this)
                .find("title")
                .text() +
              "</a></div>"
          );
        });
      });
    </script>
  </body>
</html>
```


![[Pasted image 20220617183249.png]]

