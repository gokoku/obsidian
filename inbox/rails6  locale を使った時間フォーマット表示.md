#ruby/rails 

---
2021-11-19

# locale を使った時間フォーマット表示

config / locales / ja.yml

```yaml

ja:

  date:
    formats:
      default: "%Y/%m/%d"
      long: "%Y年%m月%d日(%a)"
      short: "%m/%d"
      ja_pdf: "%Je %Jg 年 %Jm 月 %Jd 日"
      ja_pdf_column: "%Je%Jg年%Jm月%Jd日(%a)"
      ja_pdf_sign: "%Je %Jg 年 %m 月 %d 日"



  time:
    am: 午前
    formats:
      default: "%Y年%m月%d日(%a) %H時%M分%S秒 %z"
      long: "%Y/%m/%d %H:%M"
      short: "%m/%d %H:%M"
      date: '%Y/%m/%d (%a)'
      datepicker: '%Y-%m-%d'
      weekday: '(%a)'
      hhmm: '%H:%M'
      param: '%Y%m%d%H%M'
      ja_pdf: "%Je %Jg 年 %Jm 月 %Jd 日"
      ja_pdf_column: "%Je%Jg年%Jm月%Jd日(%a)"
      ja_pdf_sign: "%Je %Jg 年 %m 月 %d 日"
      long_weekday: '%A'
    pm: 午後
```


View では、I18n.l メソッドで、Lメソッド

```ruby

  # time_str.class  --> Activesupport::TimeWithZone

l(time_str, format: :date)    "2021/11/22 (月)"

l(time_str, format: :hhmm)   "09:30"
```

