#ruby/rails

---
2021-11-25

# Capybaraテスト RSpec なし

https://masuyama13.hatenablog.com/entry/2020/08/16/235800

```shell
$ rails g system_test innovation
```

test / system / innovation_test.rb

```ruby
require "application_system_test_case"

class InnovationsTest < ApplicationSystemTestCase
  # test "visiting the index" do
  #   visit innovations_url
  #
  #   assert_selector "h1", text: "Innovation"
  # end
end
```


Capybara ドキュメント

https://github.com/teamcapybara/capybara#the-dsl

```ruby
require "test_helper"

class ApplicationSystemTestCase < ActionDispatch::SystemTestCase
  driven_by :selenium, using: :chrome, screen_size: [1400, 1400]
end
```

:chrome を :headless_chrome にすればヘッドレス。

