#ruby/rails 


[Rails5でPrawnによるPDFを作成する - Qiita](https://qiita.com/sny_shinya/items/61a26ddd3f6eb2494121)

```ruby
gem 'prawn'
gem 'prawn-table'
```

```shell
$ bundle install
```

## Fontデータを用意

日本語を使えるようにするため。

<https://ipafont.ipa.go.jp/node17#jp>

ゴシックは ipaexg.ttf
明朝は ipaexm.ttg

これを app/assets/fonts/に配置した。

## PDF用クラス

```shell
$ mkdir app/pdfs
$ touch app/pdfs/request_pdf.rb
```

request_pdf.rb

```ruby
class RequestPdf < Prawn::Document
  def initialize(booking)
    super(
      page_size: 'A4',
      top_margin: 40,
      bottom_margin: 30,
      left_margin: 20,
      right_margin: 20
    )
    
    font_families.update('IPA' => {
      normal: "app/assets/fonts/ipaexm.ttf",
      bold: "app/assets/fonts/ipaexg.ttf"
    })
    
    font 'IPA'
    
    @booking = booking
    
    text "Hello!! #{@booking.id}"
  end
end
```

フォントをこのようにして登録して使うと、デフォルトでnormal、style を bold に指定して使えるようになる。

```ruby
text 'hello', style: :bold
```

## PDF コントローラ

プリントしたいモデルを保持してるコントローラにつけることにした。

```ruby
  def request_pdf
    pdf = RequestPdf(@booking)

    send_data pdf.render,
      filename: "request.pdf",
      type: "application/pdf",
      disposition: 'inline'  # 画面に表示、外すとダウンロード
  end
```

desposition を 'inline' にすると、リロードでブラウザで反映したのが見られるので、デバッグ用に便利かも。

## Routing

```ruby
  get 'booking_manager/request_pdf/:id', to: 'booking_manager#request_pdf' as: 'booking_manager_request_pdf'
```

## View link

```ruby
 = link_to 'PDF', booking_manager_request_pdf_path(@booking)
```

これでリンクを踏めば PDFが見られる。

# レイアウト

app/pdfs/request_pdf.rb を編集して、ブラウザをリロードしながらレイアウトするか。

```ruby
class RequestPdf < Prawn::Document

  def initialize(form_booking)
    super(
      page_size: 'A4',
      top_margin: 40,
      bottom_margin: 30,
      left_margin: 20,
      right_margin: 20
    )
    font 'app/assets/fonts/ipaexg.ttf'
    @form = form_booking

    stroke_axis
    
    header
    move_down 50
    content
    footer
  end
  
  def header
  end
  
  def content
  end
  
  def footer
  end
end
```

stroke_axis を入れてレイアウトの補助にできる。
