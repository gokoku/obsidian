#obsidian 

---
2021-10-04

# calendar の今日の色が見えないので変えた。


![[Pasted image 20211004160907.png]]

Appearance プラグインを使って .obsidian/snippets の中に calendar.css を作った。

```css

.today.svelte-q3wqg9 {
  --color-text-today: [[ff068e]];
}


```

chrome devtool を出して、要素のcss を見て、その変数を変えた。

color の値を直接変えても反映されない。

変数名を見つけて、それを上書きするようにするとうまくいった。

![[Pasted image 20211004161144.png]]


このままでは反映されない。

設定から Appearance をえらんで、calendar.css を on にして、リロードする。


![[Pasted image 20211004161457.png]]

OK 今日がよく分かるようになった。