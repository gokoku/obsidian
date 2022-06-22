#ruby/rails 




## 和暦gem wareki

```ruby
gem 'wareki'
```

## 日本語 ja.yml をダウンロード。

```shell
$ wget https://raw.githubusercontent.com/svenfuchs/rails-i18n/master/rails/locale/ja.yml
```

これを config/locales に設置する。

ここの date に formats があるので、追加する。

```yaml
ja:
  date:
    formats:
      ja_pdf: "%Je %Jg 年 %Jm 月 %Jd 日"
      ja_column: "%Je%Jg年%Jm月%Jd日(%a)"
```

%a で日本語曜日が出るのでそのまま使える。

## 呼び出し

```ruby
I18n.l(Date.today, format: :ja_pdf)

=> "令和 2 年 8 月 17 日"


I18n.l(Date.today, format: :ja_column)

=> "令和2年8月17日(月)"
```
