---
type: note
---

#blender

---
2022-09-05  15:22

# blender. ジオメトリーノード入門

参考

https://www.youtube.com/watch?v=4kj9n1i-JRQ

この内容を今の情報をここから取って

https://www.youtube.com/watch?v=nFKZISpDvoM

やってみた。

素晴らしいおまけ付き。

![[ノード逆引き一覧.zip]]
password : arigatou


![[Pasted image 20220905152507.png |500]]

ジオメトリノードとは、モディファイの一つのようだ。
配列の拡張版といったところが今の所の理解。

## 左のメッシュを右のメッシュの面にランダムに配置させる

適応させたいメッシュを選択して、Geometry Nodes ワークスペースで new をする。

![[Pasted image 20220905152840.png]]

![[Pasted image 20220905152916.png | 500]]

Point を面に分布するnode

![[Pasted image 20220905153108.png]]
Point にインスタンスを置く node

![[Pasted image 20220905153135.png]]

インスタンス情報をOurLiner からドラッグ&ドロップで持ってきて接続する。

![[Pasted image 20220905153438.png]]

![[Pasted image 20220905153516.png | 500]]
こうなった。ランダムに左のインスタンスを配置。
左のインスタンスを Edit モードにして scale rotate を変えるとそのまま反映される。

![[Pasted image 20220905153727.png]]

ここに他のプリミティブを追加できる。

![[Pasted image 20220905153833.png]]
キューブを追加。
Join Geometry ジオメトリ統合で合わせてoutput

![[Pasted image 20220905154124.png]]
cube のスケールを変えてこうなる。

![[Pasted image 20220905154138.png]]

## ブーリアン演算をする

Mesh Boolean を追加。

![[Pasted image 20220905154402.png | 300]]

![[Pasted image 20220905154533.png]]

こうなる。

![[Pasted image 20220905154603.png]]

これは演算なのでグニグニと変わる。

でも適応させる側を変形させるとメチャ重くなる。
使えなくなるレベルの重さ。
Boolean 演算はあかん。



