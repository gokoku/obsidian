---
type: note
---

#ruby/rails #js

---
2022-11-16  14:38

# JS で Promise を導入して非同期処理が出来た




![[Pasted image 20221116144544.png]]

canvas に画像を載せると、onload コールバックで描画するので、text を載せても上書きされる。

非同期でスマートに順次コードを書きたい。

出来た!!!

![[Pasted image 20221116145735.png]]


```js
// レンダリング
const render = async() => {
  await background()
  await title()
  await photos()
  await Promise.all( textFrames() )
  await Promise.all( texts() )
}
```
```js
// バックグラウンド画像を敷く
const background = () => {
  return new Promise((resolve, reject) => {
    const background = new Image()
    background.src = "/assets/templates/template-a01/base.jpg"
    background.onload = () => {
      context.drawImage(background, 160, 100, background.width, background.height)
      resolve()
    }
  })
}
```

```js
// タイトル
const title = () => {
  return new Promise((resolve, reject) => {
    const titleElement = document.querySelector("#template-a01-title")
    const title = titleElement.getAttribute('data-title')
    context.textAlign = "center"
    context.textBaseline = "bottom"
    context.fillStyle = "#000"
    context.font = "bold " + "180px " + fontFamily.mincho
    context.fillText(title, canvas.width/2, 600, canvas.width)
    resolve()
  })
}
```

```js
// 複数写真
const photos = () => {
  return new Promise((resolve, reject) => {
    const pElement = document.querySelector("#template-a01-photos")
    const photoPaths = pElement.getAttribute('data-photos')

    let x = 496
    photoPaths.split(',').forEach((photoPath) => {
      const photo = new Image()
      photo.src = photoPath
      photo.onload = () => {
        const width = photo.width
        const height = photo.height
        context.drawImage( photo, x, 680, 972, 834)
        x += 88 + 972
        resolve()
      }
    })
  })
}
```

複数のデータを取得して、Promise の配列を返すのがミソ。
await Promise.all( [Promise, Promise, Promise]) でまとめて待機。
```js
// テキストの下に敷く画像
const textFrames = () => {
  const frameUrls = [
    '/assets/templates/template-a01/frame01.jpg',
    '/assets/templates/template-a01/frame02.jpg',
    '/assets/templates/template-a01/frame03.jpg'
  ]
  let x = 496
  const framesPromise = frameUrls.map((frameUrl) => {
    return new Promise((resolve, reject) => {
      const frame = new Image()
      frame.src = frameUrl
      frame.onload = () => {
        context.drawImage( frame, x, 1611, 972, 886)
        x += 88 + 972

        resolve()
      }
    })
  })
  return framesPromise
}
```
テキストも同じ Promise の配列を返すようにする。

```js
// 複数テキスト
const texts = () => {
  const tElement = document.querySelector("#template-a01-texts")
  const texts = tElement.getAttribute('data-texts')
  const fontSize = pixel72to350(13)
  const lineHeight = 1.4
  const height = 750
  const width = 830

  let x = 572

  const textsPromise = texts.split(',').map((text) => {
    return new Promise((resolve, reject) => {
      context.textAlign = "left"
      context.BaseLine = "top"
      context.font = `bold ${fontSize}px ${fontFamily.gothic}`
      const lines = newLineText(text, fontSize, width)
      let y = 1730
      lines.forEach((line, i) => {
        //高さで切る
        if(i * fontSize * lineHeight > height) return
        context.fillText(line, x, y + i * fontSize * lineHeight , width)
      })
      x += 227 + width

      resolve()
    })
  })
  return textsPromise
}
```

テキストボックスを整える関数。
```js
/* テキストレンダリング用の改行で区切った文字配列変換*/
const newLineText = (text, fontSize, width) => {
  let newText = ''
  let size = 0

  for(let i = 0; i < text.length; i++) {
    if(text[i].match(/[\x21-\x80]/)) { // 半角文字の場合幅半分
      size += Math.floor(fontSize /2)
    } else if(text[i].match(/[\x20]/)){ // 半角スペースは1/4で丁度いい感じ
      size += Math.floor(fontSize /4)
    } else if(text[i].match(/\n/)) {
      //改行コードが入ってた時、その行の文字数は最初の 0 からにすること
      size = 0
    } else {
      size += fontSize
    }
    if(size > (width - fontSize)) { //行末文字で判断する
      //行末の次の文字が「。」「、」全角記号だったら、改行をここにする
      if( i+1 < text.length && text[i+1].match(/[。、]/)) {
        newText += '\n'
        size = 0
      } else {
        newText = `${newText}${text[i]}`
        newText += `\n`
        size = 0
        continue
      }
    }
    newText = `${newText}${text[i]}`
  }
  return newText.split('\n')
}
```

その他のツールとか
```js
// 72dpi の pixel を 350dpi の pixel に変換
const pixel72to350 = (pixel) => Math.round(350 * pixel / 72) - 5

// dpfファイルのダウンロード
const downloadPdf = () => {
  const img = canvas.toDataURL('image/png', 1.0)
  const doc = new jsPDF({
    orientation: "l",
    unit: "mm",
    format: [297, 210],
    compress: false
  })
  doc.addImage(img, 'png', 0, 0, 297, 210)
  doc.save('download.pdf')
  //console.log("pdf download")
}
```


レンダーする。
```js
render()
```
