#raspberry_pi   #node-red 


デフォルトでぐるぐるループ再生して、なにかイベントが入ったらその動画を1回だけ再生して、し終わったらまたデフォルト動画ループ再生になるようにした。

![](image-kn8aavev.png)


```html
<body>

    <div id="app" v-cloak>
        <div id="button">
            <button variant="primary" v-on:click="change01">01</button>
            <button variant="primary" v-on:click="change02">02</button>
            <button variant="primary" v-on:click="change03">03</button>
        </div>

        <video :src="imageSrc" :loop="loop" autoplay v-on:ended="onEnd"></video>

    </div>
```

```js
var app1 = new Vue({
  el: "#p",
  data: {
    imageSrc: "images/android.mp4",
    loop: true,
  }, // --- End of data --- //
  computed: {}, // --- End of computed --- //
  methods: {
    defaultLoop: function (event) {
      console.log("***** default **** ");
      this.imageSrc = "images/android.mp4";
			this.send()
    },
    onEnd: function (event) {
      console.log("---------- end !!!! -------------")
      this.loop = true
      this.defaultLoop()
			this.send()
    },
    change01: function (event) {
      console.log("***** change 01 **** ");
      this.loop = false
      this.imageSrc = "images/capital.mp4";
			this.send()
    },
    change02: function (event) {
      console.log("***** change 02 **** ");
      this.loop = false
      this.imageSrc = "images/sippona.mp4";
      var topic = "uibuilder/vue";
			this.send()
    },
    change03: function (event) {
      console.log("***** change 03 **** ");
      this.loop = false
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
  },
```

video のendedイベントで終了を捕まえて、デフォルトに返している。

loop = true のときは ended が発生しない。

