---
type: note
---

#rails

---
2022-11-25  18:35

# rails Ajax でコントローラを叩く

_selected_photo.html.erb
```js
    <%# クロップ機能は edit の時のみにする %>
    <% if current_page?(action: :edit) %>
      <% template = @designed_document.template %>
      <% ratio = template.photo_width / template.photo_height.to_f %>
      <script type="module">
        const cropperImg_<%= photo.id %> = document.querySelector(".modal-content.img-<%= photo.id %> img")
        const cropper_<%= photo.id %> = new Cropper(cropperImg_<%= photo.id %>, {
            viewMode: 0,
            aspectRatio: <%= ratio.round(3) %>,
            autoCropArea: 1.0,
            zoomable: false
        })
        // crop が動いたら ajax 通信する
        cropperImg_<%= photo.id %>.addEventListener('cropend', (e) => {
          const data = cropper_<%= photo.id %>.getData()
          console.log("x:",Math.round(data.x), " y: ", Math.round(data.y), " width: ", Math.round(data.width), " height: ", Math.round(data.height))
          console.log("photo_id: ", <%= photo.id %>, "designed_document: ", <%= @designed_document.id %>, "\n")
          $.ajax({
            url: '<%= crop_photo_designed_document_path(@designed_document) %>',
            type: "POST",
            data: {
              crop: {
                photo_id: <%= photo.id %>,
                crop_x: Math.round(data.x),
                crop_y: Math.round(data.y),
                crop_width: Math.round(data.width),
                crop_height: Math.round(data.height)
              },
            },
            headers: { 'X-CSRF-Token' : $('meta[name="csrf-token"]').attr('content') },
            dataType: "json"
          }).fail(function(XMLHttpRequest, status, error){
          alert(error);
          });
        })
      </script>
    <% end %>
```

cropper.js のイベントで、クロップが動いたらその値をDBに格納するようにした。

headers: で X-CSRF-Token が必要!!

## router

受け口を用意した。

config/routes.rb

```rb
  resources :design_templates
  resources :designed_documents do
    member do
      post :crop_photo
    end
  end
```

## controller

designed_documents_controller.rb
```ruby

  def crop_photo
    dd_photo = @designed_document.dd_photos.where(photo_id: params[:crop][:photo_id]).first
    dd_photo.update(crop_photos_params)
  end

private
    def crop_photos_params
      params.require(:crop).permit(:photo_id, :crop_x, :crop_y, :crop_width, :crop_height)
    end
```

DdPhoto の column name と合わせると、このようにストロングパラメータを渡すだけで良しなに入れてくれる!!!

