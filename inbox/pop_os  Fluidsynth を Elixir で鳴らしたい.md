---
type: note
---

#pop_os #elixir 

---
2023-01-10  10:13

# pop-os  Fluidsynth を Elixir で鳴らしたい

```
$ sudo apt install libfluidsynth-dev

$ iex
> Mix.install([{:midi_synth, "~> 0.4.0"}])

compile error

```

fluidsynth が見つからないと言っている。
pkg-config で見つけられないらしい。

pkg-config は ○○.pc というファイルで探すらしい。

因みに fluidsynth.pc はどこにあるかというと、
```
$ sudo find / -name '*fluidsynth.pc*'
...
/usr/lib/x86_64-linux-gnu/pkgconfig/fluidsynth.pc
```

```
$ pkg-config --libs fluidsynth
見つからない。
PKG_CONFIG_PATH に設定する

$ export PKG_CONFIG_PATH=/usr/lib/x86_64-linux-gnu/pkgconfig/:$PKG_CONFIG_PATH
$ pkg-config --libs fluidsynth
-L/usr/lib/x86_64-linux-gnu -lfluidsynth
```

これでいけるかも?!

いけた!!

```
$ iex
$ Mix.install({[:midi_synth, "~> 0.4.0"]})
:ok
```

