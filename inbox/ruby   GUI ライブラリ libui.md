#ruby

---
2021-12-14

# GUI ライブラリ libui

https://qiita.com/kojix2/items/1bf6b999973ea3bc17e6


https://github.com/kojix2/libui

```shell
$ gem install libui
```

sample.rb

```ruby
require 'libui'

UI = LibUI
UI.init

main_window = UI.new_window("Hello World", 200, 100, 1)
button = UI.new_button('Button')

UI.button_on_clicked(button) do
  UI.msg_box(main_window, "Informationd", 'You clicked the button')
end

UI.window_on_closing(main_window) do
  puts 'Bye Bye'
  UI.control_destroy(main_window)
  UI.quit
  0
end

UI.window_set_child(main_window, button)
UI.control_show(main_window)

UI.main
UI.quit

```

```shell
$ ruby sample.rb
```

![[Pasted image 20211214163445.png]]

でもなんか難しそう。
