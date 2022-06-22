#raspberry_pi  #node-red   #js/ml5 


node-red で 機械学習のチュートリアルをやってたのをやってみた。

![](image-kn9nqd80.png)

uibuilder を起点にしている。

[https://github.com/aschiffler/ml5nodered/tree/master/ml5_classificatio](https://github.com/aschiffler/ml5nodered/tree/master/ml5_classification)n

uibuilder の index.html

```html
<!doctype html>
<html lang="en">
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

    <link type="text/css" rel="stylesheet" href="../uibuilder/vendor/bootstrap/dist/css/bootstrap.min.css" />
    <link type="text/css" rel="stylesheet" href="../uibuilder/vendor/bootstrap-vue/dist/bootstrap-vue.css" />
    
    <link type="text/css" rel="stylesheet" href="./index.css" media="all">
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.9.0/p5.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.9.0/addons/p5.dom.min.js"></script>
    <script src="https://unpkg.com/ml5@latest/dist/ml5.min.js" type="text/javascript"></script>
</head>
<body>
    <div id="app" v-cloak>
        <b-container id="app_container">
            <h1>
                UIbuilder + Vue.js + bootstrap-vue for Node-RED
            </h1>
            <b-card class="mt-3" header="Control Messages" border-variant="secondary" header-bg-variant="secondary" header-text-variant="white" align="left" >
                <h1>PoseNet example using p5.js</h1>
                
                <p>
                    State Msg: <b>{{startMsg}}</b>
                </p>
                <p>Enter a URL pointing to the <code>model.json</code> or type 'MobileNet'</p>
                <p>
                    <b-form-input v-model="inputText" type="text"></b-form-input><br>
                    <b-button id="btn loadmodel" pill variant="primary" v-on:click="loadmodel">Load Model / Update</b-button>
                    <b-button id="btn setstate" pill variant="primary" v-on:click="setstate">Classification</b-button>
                </p>
                <p>Result: <b>{{resultText}}</b></p>
                <p>
                    <b-form-checkbox v-model="inputChkBox" v-on:click="setstate">Send result to node-red as</b-form-checkbox>
                </p>
            </b-card>

        </b-container>
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
    <script src="sketch.js"></script>

</body>

</html>
```

uibuilder の index.js

```jsx
'use strict'
// eslint-disable-next-line no-unused-vars
var app1 = new Vue({
    el: '#p',
    data: {
        startMsg    : 'Vue has started, waiting for messages',
        modelURL    : 'MobileNet',
        resultText  : 'nothing yet',
        feVersion   : '',
        inputText   : 'MobileNet',
        inputChkBox : false,
        stateClassification : false,
        stateModel  : false,

    }, // --- End of data --- //
    methods: {
        sendnodered: function(data) {
            uibuilder.send({'payload': data})
        },
        loadmodel: function(event) {
            this.startMsg = "Please wait Model loading..."
            this.stateClassification = false
            this.stateModel = false
            this.inputChkBox = false
            classifier = ml5.imageClassifier(this.inputText, video, modelReady)
        },
        setstate: function(event) {
            if (this.stateModel) {
                if (!this.stateClassification) {
                    this.stateClassification = true
                    classifier.classify(gotResult)
                    this.startMsg = "Camera started, Model loaded, Classification runs"
                } else {
                    this.stateClassification = false
                    this.startMsg = "Camera started, Model Loaded, Classification stopped"
                }
            }
        }
    }, // --- End of methods --- //

    // Available hooks: init,mounted,updated,destroyed
    mounted: function(){
        //console.debug('[indexjs:Vue.mounted] app mounted - setting up uibuilder watchers')
        uibuilder.start()

    } // --- End of mounted hook --- //

}) // --- End of app1 --- //

// EOF
```

uibuilder に sketch.js を追加する。p5.js

```jsx
let classifier
let video
let resultsP

function setup() {
    noCanvas()
    video = createCapture(VIDEO)

    video.size(320, 240)
    app1.startMsg = "Camera started, No model loaded, ML5.js version "    + ml5.version + " ready"
}

function modelReady() {
    if (classifier.model == null) {
        app1.startMsg = "Model loading failed check URL/path/pretrained name"
        app1.stateModel = false
    } else {
        app1.startMsg = "Camera started, Model loaded"
        app1.stateModel = true
    }
}

function classifyVideo() {
    if (app1.stateClassification) {
        classifier.classify(gotResult)
    }
}

function gotResult(err, results) {
    app1.resultText = results[0].label + ' ' + nf(results[0].confidence, 0, 2)
    console.log(JSON.stringify(results[0]))
    if (app1.inputChkBox) {
        app1.sendnodered(results[0])
    }
    //loop
    classifyVideo()
}

```

---

uibuilder でこんなページが作られる。p5.js で raspberry pi のカメラを使うために raspberry pi のブラウザで立ち上げる。


![](image-kn9nr4x8.png)

ここでモデル JSONデータを作る。

https://teachablemachine.withgoogle.com

Raspberry pi のブラウザで立ち上げて、ラズパイカメラを上げる。

![](image-kn9nrjd3.png)

モデルをエクスポートするのダウンロードでクラウドに上げる。

共有可能なリンクをコピーして + **model.json** とつけて uibuilder のURLに貼ってモデルをダウンロードさせる。

![](image-kn9nrx8z.png)

[https://teachablemachine.withgoogle.com/models/8-ybN9Y_3/model.json](https://teachablemachine.withgoogle.com/models/8-ybN9Y_3/model.json)

ここの uibuilder ページの出力を加工する。

![](image-kn9nsa8n.png)

```jsx
let result = msg.payload

msg.payload = 50

if(result.label === "Class 2" && result.confidence > 0.85) {
    msg.payload = 100
}

if(result.label === "Class 1" && result.confidence > 0.80) {
    msg.payload = 0
}

return msg
```

msg.payload の値でサーボモータの制御をしてたが、ブラウザの画面制御がうまくできなかった。

とりあえず。


