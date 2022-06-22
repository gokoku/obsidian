#ruby/rails 


form_for の time_select のいじり方

<https://github.com/rails/rails/blob/97f2c4129ae23ee074986a588628acc689a86462/actionview/lib/action_view/helpers/date_helper.rb#L665>

```ruby
f.time_select :column
```

## この後ろに付けられるもの

-   日付要らない

```ruby
ignore_date: true  
```

時間: column(4i) と 分: column(5i)だけになる
因みに 1i :年  2i :月 3i :日

-   15分刻みにしたい

```ruby
minute_step: 15
```

-   時間範囲を指定したい

```ruby
start_hour: 8, end_hour: 17
```

ここにDocあった。
<https://github.com/rails/rails/blob/375a4143cf5caeb6159b338be824903edfd62836/actionview/lib/action_view/helpers/date_helper.rb#L475-L476>

すごい。