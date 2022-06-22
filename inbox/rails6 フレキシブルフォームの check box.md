#ruby/rails 


複数選択のチェックボックスはストロングパラメータで初期値に配列を入れると自動で判断してくれる。

```ruby
   params.reauire(:user).permit( :name, :address, goods:[] )
```

先のフレキシブルフォームでは、フォーム項目を一箇所で指定するようにして記述間違いを追い出すようにしている。

が、初期値にから配列を含めたいとき一工夫する必要がある。

例えば、

```ruby
class Form::Booking


  REGISTABLE_ATTRIBUTES = {
    'day' => nil,
    'begin_time_hour' => nil,
    'begin_time_minute' => nil,
    'reservables' => []
  }
```

と Form Object で記述するとする。
reservables は \[] で初期化するようにストロングパラメータでも指定出来るようにしたい。

なので、こうするとうまくいく。

```ruby
 def form_booking_params
   params.require(:form_booking).permit(
     Form==Booking==REGISTABLE_ATTRIBUTES.symbolize_keys.map do |k, v|
       if v.nil?
         k
       else
         {k => v}
       end
    )
 end
```
