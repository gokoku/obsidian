#raspberry_pi  #node-red 


```bash
global.set( "temp", 10)

global.get( "temp" )
```

[https://qiita.com/yamachan360/items/93d530e18d2ccd62caef](https://qiita.com/yamachan360/items/93d530e18d2ccd62caef)


![](image-kndsw80y.png)

こういうことらしい。function ノードにレベルがあって、スコープが違うようだ。

それぞれ set get で使うらしい。

```bash
var localValue = 0
var contextValue = context.get('test-value')||0
var flowValue = context.get('test-value')||0
var globalValue = global.get('test-value')||0

```

