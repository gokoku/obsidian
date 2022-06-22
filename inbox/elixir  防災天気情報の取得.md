#elixir 


```shell
$ mix new weather
$ cd weather
```

mix.exs
```ruby
defp deps do
    {:httpoison, "~> 1.8"},
    {:jason, "~> 1.2"}
end
```


```shell
$ mix deps.get
```

./lib/weather.ex

```elixir
defmodule Weather do

    def get(area \\ "030000") do
        "https://www.jma.go.jp/bosai/forecast/data/overview_forecast/#{area}.json"
        |> HTTPoison.get!([], @options)
        |> Map.get(:body)
        |>
end
```

```shell
$ mix -S mix
> Weather.get
%{
  "headlineText" => "",
  "publishingOffice" => "盛岡地方気象台",
  "reportDatetime" => "2021-05-12T10:44:00+09:00",
  "targetArea" => "岩手県",
  "text" => "東北地方は高気圧に覆われています。\n\n岩手県は、晴れや曇りとなっています。\n\n１２日は、高気圧に覆われますが、湿った空気の影響により、晴れや曇りとなるでしょう。\n\n１３日は、気圧の谷の影響により、曇りや晴れで、夜遅くは雨の降る所がある見込みです。\n\n＜天気変化等の留意点＞\n１２日は、特にありません"
}
```

