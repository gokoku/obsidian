#vscode

---
2021-05-28

# vscode の Terminal で文字化けを解消する

iTerm2 で Hack Nerd Font Mono を使ってる。

フォント名を見るコマンドで Hack フォント名を確認する。

```sehll
$ fc-list : file family | grep -i hack

/Users/george/Library/Fonts/Hack Regular Nerd Font Complete Mono.ttf: Hack Nerd Font Mono
/Users/george/Library/Fonts/Hack Bold Italic Nerd Font Complete.ttf: Hack Nerd Font
/Users/george/Library/Fonts/Hack Bold Italic Nerd Font Complete Mono.ttf: Hack Nerd Font Mono
/Users/george/Library/Fonts/Hack Bold Nerd Font Complete Mono.ttf: Hack Nerd Font Mono
/Users/george/Library/Fonts/Hack Bold Nerd Font Complete.ttf: Hack Nerd Font
/Users/george/Library/Fonts/Hack Italic Nerd Font Complete.ttf: Hack Nerd Font
/Users/george/Library/Fonts/Hack Italic Nerd Font Complete Mono.ttf: Hack Nerd Font Mono
/Users/george/Library/Fonts/Hack Regular Nerd Font Complete.ttf: Hack Nerd Font
```

VSCode のSettings のここにセットする。

Terminal > Integrated: Font Family

![[Pasted image 20210528171053.png]]



## Hack Nerd Font にする

