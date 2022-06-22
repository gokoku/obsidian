---
type: note
---
セ
#ruby/rails 

---
2022-05-19  16:42

# rails 猫でもわかるHotwire入門 Turbo編 チュートリアル3

更に更に Stimulus を使ってもっと SPA にするらしい。

- 検索のインスタントサーチ
- 編集・登録のモーダル化
- ページネーション・ソート・検索時に URL が更新されるようにする
- ページネーションの無限スクロール化
- Flash の Toast化

すげーな、わくわく!!

## Stimulus
謙虚な野心を持つ JS フレームワークとのこと。
HTML から js を呼び出すらしい。
Application と Controller クラスが提供されるらしい。

[[js Hello, Stimulus]]

- Controller : data-contoller で js と紐づく
- Target : data-<コントローラー>-target  属性の値と static targets = `[]`  で定義された値が紐づく
- Action : data-action 属性の値とメソッドが紐づく


## Stimulus の debug モードを有効化
app/javascript/controlers/application.js

```js
// Configure Stimulus development experience
application.debug = true

```



## インスタントサーチの実装

入力時に検索リクエストしてくれるようにする。

Rails 7 から stimulus-rails がデフォということで、スクリプトが使えるらしい。
jsbundling-rails を使ってるので index.js にもコードが追加されるとのこと。

stimulus コントローラを作成。

```shell
$ rails g stimulus form
```

form コントローラが作られた。

app/javascript/controllers/form_controller.js

```js
import { Controller } from "@hotwired/stimulus"

// Connects to data-controller="form"
export default class extends Controller {
  connect() {
  }
}
```

app/javascript/controllers/index.js

```js
import FormController from "./form_controller.js"
application.register("form", FormController)
```

コードが追加された。


app/javascript/controllers/form_controller.js の変更。

```js
// Connects to data-controller="form"
export default class extends Controller {
  submit() {
    this.element.requestSubmit()
  }
}
```

submit() ではなく requestSubmit() にしてるのは、直でフォームの内容をリクエストしてしまって Turbo がリクエストを乗っ取れないかららしい。

app/views/cats/index.html.erb

```html
<div class="card shadow mt-3">
  <div class="card-header">
    <%= icon_with_text("search", "検索条件") %>
  </div>

  <div class="card-body">
    <%= search_form_for(
      @search,
      html: {
        data: {
          turbo_frame: "cats-list",
          controller: "form",
          action: "input->form#submit"
        },
      }
    ) do |f| %>
```

data-action の input イベントに対して、form#submit が発火して turbo-frame が更新されてるのか?! なんだか凄すぎて分からんことになってる。

search_form_for の中に data-controller と data-action を設定しただけで、インクリメントサーチが出来てしまった。

![[Pasted image 20220519180552.png| 400]]


## もう検索ボタンはいらない
app/views/cats/index.html.erb

```html
        <div class="col-4 d-flex align-items-end">
          <%# 検索結果検索フォームをクリアする %>
          <%= link_to "クリア", cats_path, class: "btn btn-outline-secondary" %>
        </div>
```


## Debounce の実装

Debounce は繰り返し実行処理されないように最後の1回だけを実行するらしい。
これで、サーバに負荷をかけないようにするとのこと。

app/javascript/controllers/form_controller.js

```js
// Connects to data-controller="form"
export default class extends Controller {
  submit() {
    // セットされている Timeout をリセット
    clearTimeout(this.timeout)

    // Timeout をセット
    // 200ms 後にリクエストを実行
    // 連続で実行されると Timeout はクリアされるので、
    // 200ms 以内のリクエストは実行されないで
    // 最後の処理だけが実行されるらしい。
    this.timeout = setTimeout(() => {
      this.element.requestSubmit()
    }, 200)
  }
}
```

this.timeout とあるが、どっから出てきたのか??


## 編集・登録のモーダル化

```shell
$ rails g stimulus modal
```

app/javascript/controllers/modal_controller.js

```js
import { Controller } from "@hotwired/stimulus"

// Bootstrap の Modal を import
import { Modal } from "bootstrap"

// Connects to data-controller="modal"
export default class extends Controller {
  connect() {
    // Bootstrap の Modal を作成
    this.modal = new Modal(this.element)

    // Bootstrap の Modal を開く
    this.modal.show()
  }

  //アクション定義
  // 保存成功時にモーダルを閉じる
  close(event) {
    // event.detail.success が true なら保存成功
    // バリデーションエラー時はモーダルを閉じないで、成功時のみ閉じる
    if (event.detail.success) {
      this.modal.hide()
    }
  }
}
```

connect() はコントローラが HTML要素にアタッチされた時(画面表示)に実行。

モーダルパーシャルを作る。

`app/views/application/_modal.html.erb`

```html
<%# `"modal"` という<turbo-frame> で囲う %>
<%= turbo_frame_tag "modal" do %>
  <%# modalコントローラをアタッチする %>
  <div class="modal fade" data-controller="modal">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title"><%= title %></h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <%= yield %>
        </div>
      </div>
    </div>
  </div>
<% end %>
```

