---
type: note
---

#rails #js

---
2022-11-24  18:22

# rails Cropper.js ライブラリを使う

```shell
$ yarn add cropperjs
$ bin/importmap pin cropperjs
```

config/importmap.rb に書かれる。

```rb
pin "cropperjs", to: "https://ga.jspm.io/npm:cropperjs@1.5.12/dist/cropper.js"
```

これは cdn 経由で読み込む仕組みらしい。

node_modules に入った cropperjs のパスを rails に指定してあげる。

config/application.rb
```ruby
    # node_modules/cropperjs の css を読み込む
    config.assets.paths << "#{config.root}/node_modules"
```

cropper.js を使えるようにする。

app/javascript/application.js に指定する
```js
import Cropper from 'cropperjs';
window.Cropper = Cropper;
```

ここまでは jsPDF と一緒だが、使うところで引っかかった。
その理由は、script type が module に指定する必要があったこと。
これがないと、Cropper は見つけられなかった。

```ruby
<script type="module">

let cropperImg_<%= photo.id %> = document.querySelector(".modal-content.img-<%= photo.id %> img")

let cropper_<%= photo.id %> = new Cropper(cropperImg_<%= photo.id %>, {
  viewMode: 0,
  aspectRatio: 4 / 3,
  zoomable: false
})
</script>
```

cropper.css を読み込む。
config/application.rb で node_modules パスを指定したので、ライブラリ名で引っ張れるようになった。

app/assets/stylesheets/application.scss
```scss
@import "cropperjs/dist/cropper.css"
```

![[Pasted image 20221124184713.png]]

出来た!!!

後は、この後、クロップ情報を取得してパラメーターにセットすると次に進めるかも。
