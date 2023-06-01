---
type: note
---

#ubuntu

---
2023-01-21  23:22

# ubuntu caps キーをctrl にする

https://linux.just4fun.biz/?plugin=attach&refer=Ubuntu%2FCaps-Lock%E3%82%AD%E3%83%BC%E3%82%92Ctrl%E3%82%AD%E3%83%BC%E3%81%AB%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95&openfile=01.png

/etc/default/keyboard

```
# KEYBOARD CONFIGURATION FILE

# Consult the keyboard(5) manual page.

XKBMODEL="pc105"
XKBLAYOUT="jp"
XKBVARIANT=""
XKBOPTIONS="ctrl:nocaps"

BACKSPACE="guess"
```

これで再起動でOK!!!!
