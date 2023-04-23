---
type: note
---

#vscode

---
2023-04-21  09:17

# vscode Prettier の設定

settings で save で検索

![[Pasted image 20230421092017.png]]


デフォルトフォーマッターを設定

format default formater で検索

![[Pasted image 20230421093339.png]]

セミコロンいらない。
今はプロジェクト毎でなくて共通でいい。

prettier で検索
![[Pasted image 20230421094931.png]]
チェック外す。




ここにリアルタイムで反映している。

![[スクリーンショット 2023-04-21 9.51.01.png]]


なんか、JSX が error になって困ったが、extention をオンオフしたら出なくなった。
どうも extention の BUG だったみたい。

```
> Operator がおかしいとか

<div> div がわからんとか
```

拡張子が .tsx になってるのを確かめる。

吐き吐き