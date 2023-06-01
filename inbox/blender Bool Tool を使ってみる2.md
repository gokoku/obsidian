#blender 


![](image-kmyggk27.png)

オブジェクトの選択順は、カッターオブジェクトを選んでから、削られるオブジェクトの順番

Auto  :   Boolean で削り取られたオブジェクトだけに一発でなる。

Brush :   Boolean はこんな感じになる。

![](image-kmygh1mq.png)

ずらせる。

Brush では Modifier に追加される。Apply で適応にする。

---

### ワイヤーフレームで適応前に確かめられる

![](2021-04-01-15-27-25-kmyhxs7e.png)

オブジェクトモードでワイヤーフレームが見られる。
どう適用されるか見られる。

この時点でトポロジーを整えられる。

Bool tools 適応で Blender が自動で入れる斜め辺を入れないように ループカット R で線を入れたのがこれ。

![](image-kmyhyevv.png)

#### 自動でマージさせて整理出来る

![](image-kmyhziic.png)

こんなところは自動でマージしてくれる Modifier が Weld

![](image-kmyi01we.png)

ここでDistance を調整すると

![](image-kmyi0msy.png)

自動でマージしてくれる。

このほうがベベルもきれいに入れられて More Better だ。

![](image-kmyi1jk9.png)

