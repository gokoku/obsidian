---
type: note
---

#blender 

---
2022-10-05  22:28

# blender.  GPU に設定したらめちゃレンダリング速くなった

preferences で Metal にして、家のmac は Radeon Pro 560X なので

![[Pasted image 20221005222946.png|500]]

まんまチェック入れて保存した。

![[Pasted image 20221005223043.png | 500]]

Devise を GPU Compute にした。

しばらくコンパイルしてたが、Cycles のレンダリングが劇的に速くなった。

![[Pasted image 20221005223250.png]]

デフォルトのままで 1分弱 だった。

