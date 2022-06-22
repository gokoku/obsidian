#raspberry_pi

---
2021-12-27

# 焦電赤外線センサーを Node-RED で見たい

センサーは　SB612A


配線は

![[Pasted image 20211217134808.png]]



Node-RED に node-red-dashboard をインストールする。

![[Pasted image 20211227170109.png]]

![[Pasted image 20211227174131.png]]

パレットは  **node-red-node-gpio**

![[Pasted image 20220307171223.png]]


## dashboard の chart で見える化

http://people3.local:1880/ui

![[Pasted image 20211227174252.png]]


超音波センサーもここに見える化したい。

### node-red のトリガーにする?

手軽だ。反応もいい。

