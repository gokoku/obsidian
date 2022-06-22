#vscode 

---
2021-12-18

# ターミナルとエディタを行き来するショートカット


キーボードのショートカット設定は左下の歯車から。

![[Pasted image 20211218194844.png]]

json 表示にして追加する。

https://zenn.dev/ganariya/articles/tmux-like-vscode

ここを見て vscode にも tmux バインドを入れようと思ったが、emacs と相性が悪いので止めた。

分割は emacs バインドでやる。

ということで vscode の editor は emacs バインド風で納めることにした。

となると、キーバインドで便利だなーと思うのは、ターミナル関係かな。

* terminal <--> editor  

    * Ctrl + Shift + \[

* editor が full になって terminal が隠れたり出たりするトグル またはその逆

    * Ctrl + Shift + \]

今の User Short Cut Json

```json
[
  {
    "key": "ctrl+x ctrl+e",
    "command": "clojureVSCode.evalAndShowResult"
  },
  // editor <--> terminal
  {
    "key": "ctrl+shift+[",
    "command": "workbench.action.terminal.focus",
    "when": "editorFocus"
  },
  {
    "key": "ctrl+shift+[",
    "command": "workbench.action.focusActiveEditorGroup",
    "when": "!editorFocus"
  },
  // toggleMaximized terminal ot editor
  {
    "key": "ctrl+shift+]",
    "command": "workbench.action.toggleMaximizedPanel",
    "when": "terminalFocus"
  },
  {
    "key": "ctrl+shift+]",
    "command": "workbench.action.togglePanel",
    "when": "editorFocus"
  },
  // terminal close
  {
    "key": "ctrl+x ctrl+k",
    "command": "workbench.action.terminal.kill",
    "when": "terminalFocus"
  },
]
```

Ctrl + Shift + ^ で terminal が new されるのが default なので、
それ近辺ということでこうしたが、terminal.focus でも new と同じ動作をするようだ。

つまり、一々 new しなくても無ければ作ってトグルする。

