#raspberry_pi  #chrome 


node-red を自動起動で立ち上げて、これを Chrome のキオスクモードで出したい。

https://qiita.com/yyano/items/81da0f56be1f5fe32781


## node-red の設定

node-red をraspberry-pi で起動時に立ち上がってる状態。
![](image-kn89z1je.png)

uibuilder があるだけ。

```html
<body>

    <div id="app" v-cloak>
        <div id="button">
            <button variant="primary" v-on:click="change00">00</button>
            <button variant="primary" v-on:click="change01">01</button>
            <button variant="primary" v-on:click="change02">02</button>
            <button variant="primary" v-on:click="change03">03</button>
        </div>
    
        <video :src="imageSrc" autoplay loop ></video>

    </div>
```

ボタンでソースを切り替えてるだけ。

やっつけでとりあえず動くのかどうなのかを見たかった。

```js
var app1 = new Vue({
	el: "#p",
	data: {
		imageSrc: "images/android.mp4",
	}, // --- End of data --- //
	methods: {
		change00: function (event) {
			console.log("***** change 00 **** ");
			this.imageSrc = "images/android.mp4";
			this.send()
		},
		change01: function (event) {
			console.log("***** change 01 **** ");
			this.imageSrc = "images/capital.mp4";
			this.send()
		},
		change02: function (event) {
			console.log("***** change 02 **** ");
			this.imageSrc = "images/sippona.mp4";
			var topic = "uibuilder/vue";
			this.send()
		},
		change03: function (event) {
			console.log("***** change 03 **** ");
			this.imageSrc = "images/blender.mp4";
			var topic = "uibuilder/vue";
			this.send()
		},
		send: function() {
			var topic = "uibuilder/vue";
			uibuilder.send({
				topic: topic,
				payload: {
					message: this.imageSrc,
				},
			});
		},
	}, // --- End of methods --- //
    
```

![](image-kn8a0vix.png)


## Chromium のキオスクモード起動
```shell
$ chromium-browser --noerrdialogs --disable-infobars --disable-bachground-mode \
--kiosk --app=http://localhost:1880/ui
```

## 止め方

これで止まる。

```shell
#!/bin/bash

killall chromium-browse
```
