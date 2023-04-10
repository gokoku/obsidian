---
type: note
---

#ruby/rails 

---
2023-03-14  14:18

# rails7  ロケール系の設定

config/application.rb

```ruby
require_relative "boot"
require "rails/all"

module YahabusGtfs
  class Application < Rails::Application

	config.load_defaults 7.0

	config.i18n.default_locale = :ja
	# I18nライブラリに訳文の探索場所を指示する
	config.i18n.load_path += Dir[Rails.root.join('config', 'locales', '**', '*.{rb,yml}').to_s]
	config.i18n.available_locales = %i[ja en]
  
	config.time_zone = 'Tokyo'
	config.active_record.default_timezone = :local
  end
end
```

config/locale/ja.yml
```yaml
ja:
  time:
    formats:
      default: "%Y/%m/%d %H:%M:%S"
      time_table: "%H時%M分"
```


時刻表時の仕方。
app/views/station_time_table.html.erb
```ryby
<%= l(stop.departure_time, format: :time_table) %>
```

![[Pasted image 20230314143454.png]]



[[rails6  locale を使った時間フォーマット表示]]


```
```