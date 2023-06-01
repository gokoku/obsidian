#raspberry_pi  #node-red 

デプロイのたび設定されてないノードがあると言われる謎。

一旦ここまでのスナップショットを取っておいて、再構築してみる。


![](image-kn8f2pz1.png)

receive from /ws-event

![](image-kn8f3385.png)

ui

index.html

```bash
<!doctype html>
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, minimum-scale=1, initial-scale=1, user-scalable=yes">

    <title>Node-RED UI Builder</title>
    <meta name="description" content="Node-RED UI Builder - VueJS + bootstrap-vue version">

    <link rel="icon" href="./images/node-blue.ico">

    <!-- See https://goo.gl/OOhYW5 -->
    <link rel="manifest" href="./manifest.json">
    <meta name="theme-color" content="[[3f51b5]]">

    <!-- Used if adding to homescreen for Chrome on Android. Fallback for manifest.json -->
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="application-name" content="Node-RED UI Builder">

    <!-- Used if adding to homescreen for Safari on iOS -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Node-RED UI Builder">

    <!-- Homescreen icons for Apple mobile use if required
        <link rel="apple-touch-icon" href="./images/manifest/icon-48x48.png">
        <link rel="apple-touch-icon" sizes="72x72" href="./images/manifest/icon-72x72.png">
        <link rel="apple-touch-icon" sizes="96x96" href="./images/manifest/icon-96x96.png">
        <link rel="apple-touch-icon" sizes="144x144" href="./images/manifest/icon-144x144.png">
        <link rel="apple-touch-icon" sizes="192x192" href="./images/manifest/icon-192x192.png">

    -->
    <link type="text/css" rel="stylesheet" href="../uibuilder/vendor/bootstrap/dist/css/bootstrap.min.css" />
    <link type="text/css" rel="stylesheet" href="../uibuilder/vendor/bootstrap-vue/dist/bootstrap-vue.css" />

    <link type="text/css" rel="stylesheet" href="./index.css" media="all">
</head>
<body>

    <div id="app" v-cloak>

        <video id="video" :src="image" autoplay @click="run()"></video>

    </div>

    <!-- These MUST be in the right order. Note no leading / -->
    <!-- REQUIRED: Socket.IO is loaded only once for all instances
                     Without this, you don't get a websocket connection -->
    <script src="../uibuilder/vendor/socket.io/socket.io.js"></script>

    <!-- --- Vendor Libraries - Load in the right order --- -->
    <script src="../uibuilder/vendor/vue/dist/vue.js"></script> <!-- dev version with component compiler -->
    <!-- <script src="../uibuilder/vendor/vue/dist/vue.min.js"></script>   prod version with component compiler -->
    <!-- <script src="../uibuilder/vendor/vue/dist/vue.runtime.min.js"></script>   prod version without component compiler -->
    <script src="../uibuilder/vendor/bootstrap-vue/dist/bootstrap-vue.js"></script>

    <!-- REQUIRED: Sets up Socket listeners and the msg object -->
    <!-- <script src="./uibuilderfe.js"></script>   //dev version -->
    <script src="./uibuilderfe.min.js"></script> <!--    //prod version -->
    <!-- OPTIONAL: You probably want this. Put your custom code here -->
    <script src="./index.js"></script>

</body>

</html>
```

index.js

```bash

"use strict";

var app1 = new Vue({
  el: "#p",
  data: {
    loopImage: "images/android.mp4",
    images: [],
    image: "images/android.mp4",
    video: null,
    msgRecvd: ""
  }, // --- End of data --- //
  watch: {
    msgRecvd: function(msg) {
      console.log(JSON.stringify(msg))
      this.images.push(msg.payload)
    }
  },
  methods: {
    send: function() {
      var topic = "uibuilder/vue";
      uibuilder.send({
        topic: topic,
        payload: {
          message: this.image,
        },
      });
    },
    loopCallBack: function(event) {
      if( this.images.length !== 0 ) {
        const src = this.images.splice(0,1)
        this.image = src[0]
        this.video.load()
      } else {
        this.image = this.loopImage
      }
      this.run()
    },
    canplayCallBack: function(event) {
      const playPromise = this.video.play()

      this.video.muted = false
      if( playPromise !== undefined ) {
        playPromise.then( () => {
          this.send()
          console.log("******************:: video")
        })
        .catch(error => {
          console.log("error: " + error)
        })
      }
    },
    run: function() {
      this.video.load()
    }
  }, // --- End of methods --- //

  // Available hooks: init,mounted,updated,destroyed
  mounted: function () {
   uibuilder.start();

    var vueApp = this;

    // Example of retrieving data from uibuilder
    vueApp.feVersion = uibuilder.get("version");

    vueApp.video = document.querySelector("#o")
    vueApp.video.addEventListener('ended', vueApp.loopCallBack)
    vueApp.video.addEventListener('canplay', vueApp.canplayCallBack)

    uibuilder.onChange("msg", function (newVal) {
      console.info('[indexjs:uibuilder.onChange] msg received from Node-RED server:', newVal)
      vueApp.msgRecvd = newVal;
    });

  }, // --- End of mounted hook --- //
}); // --- End of app1 --- //

// EOF
```

debug はみんな playload

![](image-kn8f3vja.png)

---

デバッグ系

inject

![](image-kn8f4bng.png)

Web Socket out

![](image-kn8f4ph3.png)

