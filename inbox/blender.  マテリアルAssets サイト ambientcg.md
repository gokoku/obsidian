---
type: note
---

#blender 

---
2022-09-15  09:57

# blender.  マテリアルAssets サイト ambientcg

テクスチャ配布サイト CC0 Textyres. (商用利用可能、クレジット表記不要)
https://ambientcg.com/

![[Pasted image 20220915095815.png]]

ダウンロードする。

![[Pasted image 20220915095955.png]]

色々データがダウンロードされた。



### テクスチャマッピング

Node wrangler で　Ctrl + Shift + T 


![[Pasted image 20220915100439.png]]

選択でPREVIEW 以外を選択すると、

![[Pasted image 20220915100608.png|800]]

何じゃいこりゃ!!

rough は non-color
normal は Normal Mapping を挟む
Displacement は displacement を挟む

![[Pasted image 20220915100801.png]]![[Pasted image 20220915133541.png]]

まさに質感ありじゃないですか。
何がどーなってるんだろう。

### 調べてみる

| 画像名                      | Node                       | Rrincipled BSDF input                         |
| --------------------------- | -------------------------- | --------------------------------------------- |
| Wood049_4K_Color.jpg        | Textures --> Image Texture | Base Color                                    |
| Wood049_4K_Roughness.jpg    | Textures --> Image Texture | Rouhtness                                     |
| Wood049_4K_NormalGL.jpg     | Textures --> Image Texture | Normal Map --> Normal                         |
| Wood049_4K_Displacement.jpg | Textures --> Image Texture | Displacement --> Material Output Displacement |
|                             |                            |                                               |

Color は貼り付ける画像。![[Pasted image 20220915102840.png]]

Roughness はなんだ? ![[Pasted image 20220915102906.png]]

Normal は RGB で凸凹を表してるらしい。![[Pasted image 20220915102747.png]]

Displacement がわからん。![[Pasted image 20220915102948.png]]


### Displacement Node

ジオメトリにディテールを追加するため、サーフェス法線に沿って面を変化させるとのこと。

https://docs.blender.org/manual/ja/2.90/render/shader_nodes/vector/displacement.html

Normal と合わせるといい感じになるようだ。


