---
type: note
---

#blender 

---
2022-08-05  16:45

# blender スキニングする

ボーン入れのことをスキニングというらしい。

![[Pasted image 20220805164837.png]]

この X Axis Mirror で右と左を一緒に作る。

腰から E で向かって右側にボーンを伸ばすとき、*Shift* を一緒に押すことで、左側も一度に伸びてくれる。

![[Pasted image 20220805165026.png]]

この後は、普通に E だけでいいみたい。
最初の枝分かれのところだけ Shift + E

![[Pasted image 20220805165649.png]]

曲がる方向に曲げておくのがミソとのこと。

![[Pasted image 20220805165847.png]]

大体地面につけるとのこと。

## ボーンに名前つける

![[Pasted image 20220805170552.png]]

## ウェイトを付ける

ポーズモードにすると、TAB で Edit mode と Pose mode が切り替わる。

![[Pasted image 20220805171653.png]]

まず、Outliner でこれをする。

自動でweightが付かなかったので、付ける。

Object mode で Body それから shift で ポーンを選択して、ctrl + p --> Automatic Weights

![[Pasted image 20220805172227.png]]


pose mode で確認する。

![[Pasted image 20220805172415.png]]


Edit mode で頂点単位で Weight がつくので、周りを選択してから、中心の vertex を shift 2回でアクティブにすると、Vertex Weights が Item で見られる。

![[スクリーンショット 2022-08-05 17.33.31.png]]

ここで、ボーンのweightを数字で入れる。てゆーか、余計なところに入ってる weight を 0 にしてやることをやる。

Normalize をクリックして、全部の合計が 1.0 に再割り当てしてくれる。

その後、この設定を選択してる全 Vertex にコピーしてやるのに Copy をクリックする。

