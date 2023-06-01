#vscode

---
2022-01-06

# TAB を移動するショートカット

**ctrl + TAB** でリスト検索系

右に左に移動したいのだが、

https://zenn.dev/turara/articles/vscode-move-editor-commands

自分で設定する系。


### コマンドを調べる

コマンドパレットから >move left であたりをつけて見ている。

View: Move Editor Left


### 設定ファイルを調べる

コマンドパレットから >open default

![[Pasted image 20220106105228.png]]

このファイルを開いて、MoveEditorLeft で検索する。


どうもやりたかったものは、workbench.action.nextEditor のようだ。

これは元からあるので、これを使う。

**alt + ⌘ + →**

**alt + ⌘ + ←**

**shift + ⌘ + [   、  shift + ⌘ + ] **
















