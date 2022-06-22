#vscode 

---
2022-03-09  20:48

# vscode  Ruby Debugger vscode-rdbg

![[Pasted image 20220309204826.png]]

ruby 3.1 では 標準で入ってる debug

```shell
$ gem install debug
```

![[Pasted image 20220309205114.png]]

デバッガで最強は REPL だな。
あまり使い勝手がよくわからん。


**![[Pasted image 20220309205858.png]]

左から
- 続行             : 中断したところから実行を続行
- Step Over   : 現在の行を実行して、次の行で実行停止
- Step In        : 現在の行を実行して、次の行で実行停止、その行が関数呼び出しならそこに入って停止
- Step Out    : 実行している関数が終了するまでステップ実行を継続
- 再起動
- 終了


