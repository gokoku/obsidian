#js

---
2021-09-08

# ガルーン Garoon の検証

[Garoon の実験場](https://p-demo.cybozu.com/g/)

## 実験室の設置

+Add My Portal でポータルを追加する。

![[Pasted image 20210908134000.png]]

HTML portlets を作る。これを js ランチャーとして使う。

![[Pasted image 20210908134123.png]]

![[Pasted image 20210908134242.png]]

JavaScript and CSS customization でこれに js を付ける。

js コードをアップロードする。

```js
const msg = document.querySelector('[[send-message]]')
msg.addEventListener('click', () => {
  console.log('Hello, world!')
})
```


Apply をチェックすること。

![[Pasted image 20210908135410.png]]


ポータルセッティングで作ったポートレイットを配置する。

![[Pasted image 20210908135129.png]]

ポータルを開く。

![[Pasted image 20210908135159.png]]

これで、js がクリックで発火して動くことがわかった。

![[Pasted image 20210908135630.png]]


## メッセージを自分に送信するには

SOAP を使う。

user_id と login_name が判れば、メッセージを送れる。

```js
var xxx
;(function () {
  const msg = document.querySelector('[[send-message]]')
  msg.addEventListener('click', () => {
    sendMessage((err, resp) => {
      if (err) {
        alert(err)
        return
      }
      const data = resp[0].children[0] // <th:addressee .../>
      const user_id = data.attributes['user_id']
      const name = data.attributes['name']
      console.log(user_id.value, name.value) // 送り先名
    })
  })

  // soap xml を作成する
  var makeXMLHeader = function (action, user_id, login_name) {
    return (
      '<?xml version="1.0" encoding="UTF-8"?>' +
      '<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">' +
      '<soap:Header>' +
      '<Action>' +
      `${action}` +
      '</Action>' +
      '<Security>' +
      '<UsernameToken>' +
      '<Username>tada</Username>' +
      '<Password>Cpu68060</Password>' +
      '</UsernameToken>' +
      '</Security>' +
      '<Timestamp>' +
      '<Created>2010-08-12T14:45:00Z</Created>' +
      '<Expires>2037-08-12T14:45:00Z</Expires>' +
      '</Timestamp>' +
      '<Locale>jp</Locale>' +
      '</soap:Header>' +
      '<soap:Body>' +
      `<${action}>` +
      '<parameters>' +
      '<create_thread>' +
      '<thread id="dummy" version="dummy" subject="メッセージテスト" confirm="false">' +
      `<addressee user_id="${user_id}" name="${login_name}" deleted="false"></addressee>` +
      '<content body="これはテストです。"></content>' +
      '<folder id="dummy"></folder>' +
      '</thread> ' +
      '</create_thread>' +
      '</parameters>' +
      `</${action}>` +
      '</soap:Body>' +
      '</soap:Envelope>'
    )
  }

  const sendMessage = function (callback) {
    const xmlhttp = new XMLHttpRequest()
    const url = '/g/cbpapi/message/api.csp'
    let respText, resp
    //const xml = makeXMLHeader('MessageCreateThreads', '86', 'tada')
    const xml = makeXMLHeader('MessageCreateThreads', '85', 'mirai')

    xmlhttp.open('post', url, true)
    xmlhttp.setRequestHeader('Content-Type', 'text/xml; charset=UTF-8')
    xmlhttp.setRequestHeader('X-Requested-With', 'XMLHttpRequest')
    xmlhttp.setRequestHeader('mode', 'no-cors')
    xmlhttp.setRequestHeader('credentials', 'include')

    xmlhttp.onload = function (e) {
      if (xmlhttp.readyState === 4) {
        if (xmlhttp) {
          respText = xmlhttp.responseText
          resp = $.parseXML(respText)
          callback(null, $(resp).find('thread'))
        } else {
          callback(xmlhttp.statusText)
        }
      }
    }
    xmlhttp.onerror = function (e) {
      callback(xmlhttp.statusText)
    }
    xmlhttp.send(xml)
  }
})()
```

## user_id とログイン名を取得するには

組織情報一覧を取得する。

```js

const body = { limit: 200 }

garoon.api(
  '/api/v1/base/organizations',
  'GET',
  body,
  (res) => {
    const organizations = res.data.organizations
    console.log(organizations)
  },
  (error) => {
    console.log(error)
  },
)

//組織の一覧

// [1]:営業部本部

// [2]:事業所本部

// [12]:システムサービス部

// [13]:全社員

// [16]:テスト組織 ....
```

組織 id がわかれば、所属するメンバー一覧が取得できて、user_id と login_name がわかる。

これで、メッセージを送信出来る。

```js
//[1]: 営業部本部
garoon.api(
  '/api/v1/base/organizations/1/users',
  'GET',
  body,
  (res) => {
    const members = res.data.users
    console.log(members)
    members.forEach((element) => {
      const li = document.createElement('li')
      const { id, name, code } = element
      console.log(`id: ${id}  name: ${name}   login_name: ${code}`)
    }
  },
  (error) => {
    console.log(error)
  },
)
```




## Garoon Utility for JavaScript なるものがある
https://github.com/garoon/garoonUtility

garoon Utility for JavaScriptはサイボウズ ガルーンのSOAP APIにアクセスするJavaScript ライブラリです。  
SOAP APIのリクエストやレスポンスをjson形式で取り扱うことができます。とのこと。

https://developer.cybozu.io/hc/ja/articles/115001805783

dist/garoonUtility.min.js を Garoon の js にアップロードする。

![[Pasted image 20210915110903.png]]




## メッセージにファイルを添付するには

## ファイルをダウンロードする


```js
const soap_template = (action, create, parameters) =>
  '<?xml version="1.0" encoding="UTF-8"?>' +
  '<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">' +
  '<soap:Header>' +
  `<Action>${action}</Action>` +
  '<Timestamp>' +
  `<Created>${create}</Created>` +
  '<Expires>2037-08-12T14:45:00Z</Expires>' +
  '</Timestamp>' +
  '<Locale>jp</Locale>' +
  '</soap:Header>' +
  '<soap:Body>' +
  `<${action}>` +
  `${parameters}` +
  `</${action}>` +
  '</soap:Body>' +
  '</soap:Envelope>'

/**
 * ファイルのダウンロード Base64 で取得する
 */
var downloadFile = function (fileId) {
  var defer = $.Deferred()

  const time = moment().add(-9, 'hours').format('YYYY-MM-DDTHH:mm:ssZ')
  const params = `<parameters file_id="${fileId}"></parameters>`
  const request = soap_template('CabinetFileDownload', time, params)

  $.ajax({
    type: 'post',
    url: '/g/cbpapi/cabinet/api.csp?',
    cache: false,
    async: true,
    data: request,
  }).then(function (resonse) {
    defer.resolve($(resonse).find('content').text())
  })
  // 本来はエラー処理を実施

  return defer.promise()
}

const btn = document.querySelector('#t')
btn.addEventListener('click', () => {
  downloadFile(385).then((res) => {
    console.log(res)
  })
})
```

fileId は fid のことで、hid(フォルダーid) は要らないようだ。

これで本当に 指定した pdf ファイルがダウンロードされるのだろうか。

console.log で Base64 らしきところから直にコピーして、data.txt に貼り付ける。
何故か右クリックの保存するだと、うまくいかなかったので、直に選択して右クリックコピーする。

```shell
$ base64 -d data.txt > data.pdf

$ open data.pdf
```

![[Pasted image 20210916172806.png]]

これがそれです。バッチリでした!!!


chrome で console.log は途中だったりする。more ... をクリックして全部出す。

Base64 の終わりは == なので、そこをチェックする。

