#js

---
2021-09-24

# ファイルを Base64 でロードする

複数のファイルをロード出来る。

![[Pasted image 20210924180336.png]]

```html
<input type="file" onchange="uploadFiles(this)" multiple>
```

```js
const uploadFiles = (input) => {
  ;[].forEach.call(input.files, (file) => {
    if (/\.(pdf)$/i.test(file.name)) {
      const reader = new FileReader()

      reader.addEventListener('load', (event) => {
        console.log(file.name)
        console.log(event.target.result)
      })
      reader.readAsDataURL(file)
    }
  })
}
```

console で Base64 が見れる。

Show more をクリックして全部表示してコピペする。

頭の data:application/pdf;base64, を削除する。

```text
data:application/pdf;base64,JVBERi0xLjYNJeLjz9MNCjEgMCBvYmoNPDwvTWV0YWRhdGEgMiAwIFIvT0NQcm9w
```
↓
```text
JVBERi0xLjYNJeLjz9MNCjEgMCBvYmoNPDwvTWV0YWRhdGEgMiAwIFIvT0NQcm9wZXJ0aWVzPDwvRDw8L09OWzYgMCBSIDI4
```


pdf に変換する。

```shell
$ base64 -d test.txt -o test.pdf

$ open test.pdf
```

元通り。
