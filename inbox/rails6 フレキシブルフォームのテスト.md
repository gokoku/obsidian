#ruby/rails 


![](1d4ea805-0804-4150-9f79-916d023ac25b-klwd97rb.png)


フレキシブルフォームの ActiveModel のテストは入り口と出口だけのチェックで簡単にすますことにした。



## セットアップ

ActiveModel のテストフレームワークはこのようにするとのこと。
これでだいたい ActiveRecord と同じテストが出来るようになる。

```ruby
require 'test_helper'

class Form==BookingTest < ActiveSupport==TestCase
  include ActiveModel==Lint==Tests

  setup do
    @params = {
      "reserve_day" =>  "2020-07-28",
      "begin_time_hour" => "19",
      "begin_time_minute" => "30",
      "end_time_hour" => "19",
      "end_time_minute" => "30",
      "organization" => "people",
      "delegate" => "people",
      "responsible_party" => "taro",
      "address" => "巣子",
      "tel" => "00000001111",
      "fax" => "00000001111",
      "number" => "10",
      "purpose" => "ok",
      "reservables" => ["1", "2", "3"]
    }

    @model = Form::Booking.new(@params)
  end
end
```

## 入り口　バリデーションチェック

invalid な値をセットして new して invalid か聞いてみる形

```ruby
  test "validate check" do
    @params["reserve_day"] = nil
    @model = Form::Booking.new(@params)

    assert @model.invalid?
  end
```

## 出口　JSONチェック

### save チェック

値のハッシュ配列値と、save 先の Form モデルの data カラムをパースしたものを比較するとチェックになるとする。

```ruby
  test "save" do
    @model.save

    hash = JSON.parse Form.last.data
    assert hash == @params
  end
```

### update チェック

値を変えて書き換わってるかをチェックする。

```ruby
  test "update" do
    @model.save
    change_purpose = "hoge hoge"
    @model.purpose = change_purpose
    @model.update(Form.last)

    get_json = Form.last.data
    assert JSON.parse(get_json)["purpose"] == change_purpose
  end
```

# 

# code

```ruby
require 'test_helper'

class Form==BookingTest < ActiveSupport==TestCase
  include ActiveModel==Lint==Tests

  setup do
    @params = {
      "reserve_day" =>  "2020-07-28",
      "begin_time_hour" => "19",
      "begin_time_minute" => "30",
      "end_time_hour" => "19",
      "end_time_minute" => "30",
      "organization" => "people",
      "delegate" => "people",
      "responsible_party" => "taro",
      "address" => "巣子",
      "tel" => "00000001111",
      "fax" => "00000001111",
      "number" => "10",
      "purpose" => "ok",
      "reservables" => ["1", "2", "3"]
    }
    @model = Form::Booking.new(@params)
  end

  test "should get columns" do
    assert_equal @params["reserve_day"], @model.reserve_day
    assert_equal @params["reservables"], @model.reservables
  end

  test "validate check" do
    @params["reserve_day"] = nil
    @model = Form::Booking.new(@params)

    assert @model.invalid?
  end

  test "save" do
    @model.save

    hash = JSON.parse Form.last.data
    assert hash == @params
  end

  test "update" do
    @model.save
    change_purpose = "hoge hoge"
    @model.purpose = change_purpose
    @model.update(Form.last)

    get_json = Form.last.data
    assert JSON.parse(get_json)["purpose"] == change_purpose
  end

end

```
