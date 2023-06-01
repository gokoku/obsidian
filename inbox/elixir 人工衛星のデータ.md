---
type: note
---

#elixir

---
2023-03-22  10:18

# 人工衛星のデータ

http://celestrak.org/NORAD/elements/

NORAD GP Element Sets Current Data 

ガリレオとかある。

みちびきとかどれだろう。

ガリレオの軌道を見てみたいのだが。時間軸で位置が計算出来るってことだろうか。

https://qiita.com/ciscorn/items/80b3a3f526316f78b24a

これがやりたい。elixir livebook で出来ないだろうか。

https://qiita.com/yonda_gliese/items/7fe9a9beaf78fd7f1282
えっjsで出来るの?


## livebook
```elixir
Mix.install([
  {:explorer, "~> 0.5"},
  {:geo, "~> 3.4"},
  {:kino, "~> 0.8"},
  {:kino_maplibre, "~> 0.1.3"},
  {:download, "~> 0.0.4"}
])
```

```elixir
"http://celestrak.org/NORAD/elements/gp.php?GROUP=stations&FORMAT=json"
|> HTTPoison.get!()
|> Map.get(:body)
|> Jason.decode!()
|> Kino.DataTable.new()
```

![[Pasted image 20230322172736.png]]

OBJECT_NAME が人工衛星の名前?
ISS って一杯ね?

さあ、このデータは何だ? 
