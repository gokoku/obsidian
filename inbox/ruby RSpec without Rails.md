#mac 



ruby スクリプトでもテストがしたい。

RSpec を使うことにした。

```shell
$ bundle init
```

Gemfile

```ruby
gem 'rspec', "~> 3.9"
```

```shell
$ bundle install
```

### rspec の初期化

<https://qiita.com/yusabana/items/db44b81bdddf6ed0e9f5>

```shell
$ bundle exec rspec --init
```

spec/spec_helper.rb の =begin =end を外す。

### rspec テストの書き方

```ruby
require_relative "../hoge"    # hoge.rb を呼び出す

RSpec.describe "hello" do     # 文字列じゃないときはクラス名ととるようだ
  it "sample test" do
    expect(hoge).to eq "hello"
  end
end
```

### テストしやすい script にする

手続き型だと require で上から順番に実行してしまう。
