#elixir 

Jupyter notebook みたいなものがある。

https://github.com/elixir-nx/livebook
![](logo-with-text-kojlw18d.png)

```shell
$ git clone https://github.com/livebook-dev/livebook.git
$ cd livebook
$ mix deps.get --only prod


# Run the Livebook server
$ MIX_ENV=prod mix phx.server
```

http://localhost:8080/?token.... と出るから

新規ノート作成

![](image-kojm9dlk.png)

保存はどーしよー
![](image-kojm756l.png)

この livebook の中に notebook フォルダー作ってその中にした。

![](image-kojmdbjn.png)



ctrl-c ctrl-c で終了した。


### Mix install
```ruby
Mix.install([
    {:exla, "~> 0.1.0-dev", github: "elixir-nx/nx", sparse: "exla"}
])

require exla
```


### ExUnit test

モジュール
```ruby
defmodule Hello do
    def world do
        "hello world!"
    end
end
```

テスト

```ruby
ExUnit.start(autorun: false)

defmodule HelloTest do
    use ExUnit.Case, async: true
    
    test "it workd" do
        assert Hello.world() == "hello world!"
    end
end
ExUnit.run()
```

あかん。終わらん。