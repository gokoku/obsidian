---
type: note
---

#raspberry_pi 

---
2023-02-09  15:26

# 遠野アバターのまとめ Node-RED

![[Pasted image 20230209152721.png]]

![[tonoabator 2023-02-09 14.41.30.excalidraw|800]]


ボタンをクリックする

—> WebSocket で click 情報(今・ココのモード番号)を送信する

—> 受信した情報でモード番号をセット(前クライアントのモード番号同期)して更新処理をする

顔認識したら Websocket を送信する

—> もしモードが0番だった場合だけ更新処理をする


## js 統括コントローラー

![[スクリーンショット 2023-02-09 15.51.13.png]]

### 状態遷移で制御する

mode 0

![[Pasted image 20230209155827.png|300]]

mode 1
![[Pasted image 20230209155944.png|300]]

mode2
![[Pasted image 20230209164657.png|300]]

![[Pasted image 20230209164622.png]]


### コードに落とす

Avator オブジェクトを作り、この中で状態が mode で遷移することにした。
各モードごとに更新する情報が配列でセットされるようにして、同じ更新メソッドを使って動画、パネル、状態表示が切り替わるようにした。

ただし、各モードに遷移済みの場合だけ次への遷移を受け付けるようにしたかった。
更新処理が始まって終わるまでイベント入力をブロックできるように、enable と disable の状態を持たせてコントロールすることで実現できた。

各処理は非同期で行われるので、非同期プログラミングで実装して安定して動作できるようにした。

```js

'use strict'

class Avator {
    constructor(modeObj) {
        this.mode = 0
        this.isEnable = true
        this.enable = this.enable.bind(this)

        this.buttons = modeObj.map((obj) => obj.button )
        this.next_srces = modeObj.map((obj) => obj.next_src)
        this.next_statuses = modeObj.map((obj) => obj.next_status)
        this.initial_status = this.next_statuses[this.next_statuses.length - 1]
    }
    init() {
        this.mode = 0
        this.enable()
        $('.main >p.state').text(this.initial_status)
    }
    modeGet() {
        return this.mode
    }
    modeSet(i) {
        this.mode = i
        console.log("mode: ", this.mode)
    }
    async next() {
        if(this.isEnable) {
            await this.disable()
            await this.exec(this.next_srces[this.mode],
             this.next_statuses[this.mode])
            this.mode = await (this.mode + 1) % this.buttons.length
        }
    }

    disable() {
        //見た目を全部disable
        $("ul.mode > li").removeClass()
        $(this.buttons).each((_index, element) => {
            $(element).addClass("non-active")
        });
        //イベントをdisable
        this.isEnable = false
    }
    enable() {
        //見た目をモードのところだけenable
        $("ul.mode > li").removeClass()
        $(this.buttons).each((index, element) => {
            if(index == this.mode) {
                $(element).addClass("active")
            } else {
                $(element).addClass("non-active")
            }
        })
        //イベントを受け付ける
        this.isEnable = true
    }
    
    exec = (src, status) => {
        const video = $('video#greeting')
        video.attr('src', src)
        $(video).on('canplay', (e) => {
            playVideo(video)
            $('.main').fadeOut("fast", () => {
                $(video).on('ended', (e) => {
                    $('.main >p.state').text(status)
                    $('.main').fadeIn("slow", () => {
                        console.log("exec: ", status)
                        this.enable()
                        $(video).off('ended')
                    })
                })
            })
            $(video).off('canplay')
        })
    }
}

async function playVideo(videoElm) {
    try {
        await videoElm.trigger('play')
        console.log("play video")
    } catch (err) {
        console.log(err)
    }
}


// Send a message back to Node-RED
window.fnSendToNR = function fnSendToNR(payload) {
    uibuilder.send({
        'topic': 'click',
        'payload': payload,
    })
}

// run this function when the document is loaded
window.onload = function() {
    // Start up uibuilder - see the docs for the optional parameters
    uibuilder.start()

    const avator = new Avator(
        [
            {"button": $('li#mode-wait'),
            "next_src": "./Media/avator01-greet1.mp4",
            "next_status": "対応する"},
            
            {"button": $('li#mode-greet1'),
            "next_src": "./Media/avator01-greet2.mp4",
            "next_status": "対応中"},
            
            {"button": $('li#mode-greet2'),
            "next_src": "./Media/avator01-finish.mp4",
            "next_status": "受付中"}
        ]
    )
    // button event setting
    $('ul.mode > li').on('click tap', () => {
        // send to node-red
        window.fnSendToNR({"mode": avator.modeGet()})
    })
    // reset button setting
    $('button#reset').on("click tap", () => {
        // send to node-red
        window.fnSendToNR({"reset": 0})
    })

    // Listen for incoming messages from Node-RED
    uibuilder.onChange('msg', function(msg){
        const payload = JSON.parse(msg.payload)
        if(msg.topic == "click") {
            if(payload.reset == undefined) {
                // payloadからモードを取得してセットすることに
                // したので全クライアントが同期するようになった
                avator.modeSet(payload.mode)
                avator.next()
            } else {
                // リセットの時
                avator.init()
            }
        }
        if(msg.topic == "face") {
            // 待機モードの時だけ反応する
            if(avator.modeGet() == 0) {
                avator.next()
            }
        }
    })
}
```

状態遷移を動かすのが WebSocket によるデータの送信である。

更新処理には動画の終了までが含まれるので、処理中は受信をブロックすることにしている。

ボタンをクリック出来るのが全体の制御を出来るようにしたので、顔認識しなくても受付中のボタンをクリック出来る。

リセットボタンで全ブラウザでが初期状態に出来るようにした。

![[Drawing 2023-02-09 16.30.12.excalidraw | 700]]

