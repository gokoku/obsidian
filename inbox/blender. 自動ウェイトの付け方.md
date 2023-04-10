---
type: note
---

#blender

---
2022-08-02  11:52

# blender. 自動ウェイトの付け方

オブジェクトモードでボディ + shift + ボーンで選択して、ctrl + p で Automatic Weights を選択する。


![[Pasted image 20220802115207.png]]


ボディの頂点をセレクトすると
![[Pasted image 20220802125958.png]]

Vertex Widgets が現れた。

---
周りを選択して、shift + 2回クリックで、 Vertex Weights の値を調整する。
例えば、Upperleg を 0 にして影響をなくする。

その後で、Normalize で、合計が 1.0 になるようにする。
その後で、選択してる他の vertex に反映させるために Copy をクリック。

これを繰り返す。

![[Pasted image 20220802143917.png]]

ウェイト分離できた。

