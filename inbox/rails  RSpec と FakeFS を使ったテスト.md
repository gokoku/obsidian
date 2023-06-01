#ruby/rails


rails 以外の ruby script で RSpec を使ったファイル系テストに FakFS を使うと便利だった。

# 準備

Gemfile

```ruby
gem 'rspec', "~>3.9"
gem 'fakefs'
```

```shell
$ bundle exec rspec --init
```

spec_helper.rb

```ruby
require 'fakefs/spec_helpers'
RSpec.configure do |config|
  config.include FakeFS::SpecHelpers
```

# 使ってみる

## ディレクトリ一覧を取得するケースのテスト

xml_merger.rb

```ruby
######################
# XMLファイルリスト取得
######################
def xml_list(dir, reg_func)
  xml_list = []
  Dir.chdir(dir) do
    Dir.glob("*.xml") do |fname|
      xml_list << fname if reg_func.call(fname, "exist")
    end
  end
  xml_list
end
```

これのテストがしたい。
FakeFS で偽ディメクトリとその中の偽ファイルを用意して、その結果を期待通りかどうかテストした。

xml_list_spec.rb

```ruby
require_relative '../xml_merger'

RSpec.describe "xml_listにおいて" do

  it "指定ディレクトリから該当ファイル名のリストを取得できること" do
    files = [
      "20201010-00000001-MIT-1.xml",
      "20201010-00000002-MIT-1.xml",
      "20201010-00000003-MIT-1.xml",
      "20201010-00000004-MIT-del.xml",
      "20201010-00000005-MIT-1.xml",
      "0000000.xml",
      "example.xml"
    ]

    FileUtils.mkdir "test_dir"
    Dir.chdir("test_dir") do
      files.each {|f| FileUtils.touch f }
    end

    expect(xml_list("test_dir", method(:extract_reg))).to eq [
      "20201010-00000001-MIT-1.xml",
      "20201010-00000002-MIT-1.xml",
      "20201010-00000003-MIT-1.xml",
      "20201010-00000004-MIT-del.xml",
      "20201010-00000005-MIT-1.xml"
    ]
  end
end
```

## ファイルの中身をテストするケース

```ruby
######################
# ファイルのマージ処理
######################
def xml_merge(xml_list, read_dir, write_dir, output)
  open(write_dir + output, 'w') {}

  xml_list.each_with_index do |list, file_i|
    open(read_dir + list, 'r').each_line.with_index do |row, row_i|
      next if file_i.nonzero? && row_i.zero?
      open(write_dir + output, 'a') {|f| f << row}
    end
    open(write_dir + output, 'a') {|f| f << "\n\n"}
  end
end
```

読み出しファイルと書き出しファイルをニセモノで用意した。
簡単に中身のあるファイルを作れて、書き出したファイルも簡単にアクセス出来る。

とても直感的で気にいにました。
これは日常系の ruby スクリプトでも手軽に使える。

```ruby
require_relative '../xml_merger'

RSpec.describe "xml_mergeにおいて" do

  it "XMLリストで指定されたXMLを指定通りマージして書き出しが出来ること" do
    xml_list = [
      "20201010-00000001-MIT-2.xml",
      "20201010-00000002-MIT-1.xml",
      "20201010-00000003-MIT-1.xml",
    ]

    test_dir = "test_dir/"
    out_dir = "out_dir/"
    out_file = "write.xml"

    FileUtils.mkdir test_dir
    FileUtils.mkdir out_dir

    Dir.chdir(test_dir) do
      xml_list.each do |list|
        File.open(list, 'w') do |f|
          f.write [
            '<?xml version="1.0" encoding="UTF-8"?>',
            '<YJNewsFeed Version="1.0">',
            list,
            '</YJNewsFeed>'
          ].join("\n")
        end
      end
    end

    xml_merge(xml_list, test_dir, out_dir, out_file)

    str = <<-EOS
<?xml version="1.0" encoding="UTF-8"?>
<YJNewsFeed Version="1.0">
#{xml_list[0]}
</YJNewsFeed>

<YJNewsFeed Version="1.0">
#{xml_list[1]}
</YJNewsFeed>

<YJNewsFeed Version="1.0">
#{xml_list[2]}
</YJNewsFeed>

    EOS

    expect(File.read(out_dir + out_file)).to eq str

  end
end
```
rails 以外の ruby script で RSpec を使ったファイル系テストに FakFS を使うと便利だった。

# 準備

Gemfile

```ruby
gem 'rspec', "~>3.9"
gem 'fakefs'
```

```shell
$ bundle exec rspec --init
```

spec_helper.rb

```ruby
require 'fakefs/spec_helpers'
RSpec.configure do |config|
  config.include FakeFS::SpecHelpers
```

# 使ってみる

## ディレクトリ一覧を取得するケースのテスト

xml_merger.rb

```ruby
######################
# XMLファイルリスト取得
######################
def xml_list(dir, reg_func)
  xml_list = []
  Dir.chdir(dir) do
    Dir.glob("*.xml") do |fname|
      xml_list << fname if reg_func.call(fname, "exist")
    end
  end
  xml_list
end
```

これのテストがしたい。
FakeFS で偽ディメクトリとその中の偽ファイルを用意して、その結果を期待通りかどうかテストした。

xml_list_spec.rb

```ruby
require_relative '../xml_merger'

RSpec.describe "xml_listにおいて" do

  it "指定ディレクトリから該当ファイル名のリストを取得できること" do
    files = [
      "20201010-00000001-MIT-1.xml",
      "20201010-00000002-MIT-1.xml",
      "20201010-00000003-MIT-1.xml",
      "20201010-00000004-MIT-del.xml",
      "20201010-00000005-MIT-1.xml",
      "0000000.xml",
      "example.xml"
    ]

    FileUtils.mkdir "test_dir"
    Dir.chdir("test_dir") do
      files.each {|f| FileUtils.touch f }
    end

    expect(xml_list("test_dir", method(:extract_reg))).to eq [
      "20201010-00000001-MIT-1.xml",
      "20201010-00000002-MIT-1.xml",
      "20201010-00000003-MIT-1.xml",
      "20201010-00000004-MIT-del.xml",
      "20201010-00000005-MIT-1.xml"
    ]
  end
end
```

## ファイルの中身をテストするケース

```ruby
######################
# ファイルのマージ処理
######################
def xml_merge(xml_list, read_dir, write_dir, output)
  open(write_dir + output, 'w') {}

  xml_list.each_with_index do |list, file_i|
    open(read_dir + list, 'r').each_line.with_index do |row, row_i|
      next if file_i.nonzero? && row_i.zero?
      open(write_dir + output, 'a') {|f| f << row}
    end
    open(write_dir + output, 'a') {|f| f << "\n\n"}
  end
end
```

読み出しファイルと書き出しファイルをニセモノで用意した。
簡単に中身のあるファイルを作れて、書き出したファイルも簡単にアクセス出来る。

とても直感的で気にいにました。
これは日常系の ruby スクリプトでも手軽に使える。

```ruby
require_relative '../xml_merger'

RSpec.describe "xml_mergeにおいて" do

  it "XMLリストで指定されたXMLを指定通りマージして書き出しが出来ること" do
    xml_list = [
      "20201010-00000001-MIT-2.xml",
      "20201010-00000002-MIT-1.xml",
      "20201010-00000003-MIT-1.xml",
    ]

    test_dir = "test_dir/"
    out_dir = "out_dir/"
    out_file = "write.xml"

    FileUtils.mkdir test_dir
    FileUtils.mkdir out_dir

    Dir.chdir(test_dir) do
      xml_list.each do |list|
        File.open(list, 'w') do |f|
          f.write [
            '<?xml version="1.0" encoding="UTF-8"?>',
            '<YJNewsFeed Version="1.0">',
            list,
            '</YJNewsFeed>'
          ].join("\n")
        end
      end
    end

    xml_merge(xml_list, test_dir, out_dir, out_file)

    str = <<-EOS
<?xml version="1.0" encoding="UTF-8"?>
<YJNewsFeed Version="1.0">
#{xml_list[0]}
</YJNewsFeed>

<YJNewsFeed Version="1.0">
#{xml_list[1]}
</YJNewsFeed>

<YJNewsFeed Version="1.0">
#{xml_list[2]}
</YJNewsFeed>

    EOS

    expect(File.read(out_dir + out_file)).to eq str

  end
end
```

