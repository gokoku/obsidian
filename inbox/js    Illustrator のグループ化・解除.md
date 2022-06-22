#js #illustrator



[イラストレーター：Javascriptでのグループ化とグループ解除する関数 - なにする？DTP+WEB](https://kamiseto.hatenadiary.org/entry/20091103/1257222929)

これはUtility としてかなり便利に使える。

```js
//グループ解除
function removeGroup(GP){
	if(GP.constructor.name !== 'GroupItem')return false;
	var gpItems = GP.pageItems;
	var moveItems =[];
	for(i=0;i<gpItems.length;i++){
				moveItems.push(gpItems[i]);
	}
	while(moveX = moveItems.shift()){
				moveX.move(GP,ElementPlacement.PLACEBEFORE);		
	}
	GP.remove();
	return true
}
```

```js
//グループ化
function makeGroup(ITEM){
	var ITEMSArray= [];	
	ITEM.constructor.name !== 'Array' ? ITEMSArray.push(ITEM) : ITEMSArray = ITEM;
	var NEWGP = app.activeDocument.groupItems.add();
	NEWGP.move(ITEMSArray[0],ElementPlacement.PLACEBEFORE);
	for(i=0;i<ITEMSArray.length;i++){
		ITEMSArray[i].move(NEWGP,ElementPlacement.PLACEATEND);
	}
	return true;
}
```
