#js

---
2021-08-31

# 3つのやりかた

https://into-the-program.com/execution-firsttime-access/

Cookie は抵抗あるかな。

sessionStrage はブラウザを閉じたらクリアらしい。

2度と表示しないようにするには localStorage のようだ。


## localStrage を使ってみる

reload のときにとあるキーを押してると、localStrage がクリアするようにしたい。

```js
// localStrage に訪問 state を保持(Boolean ではなく String でしか保存されないので注意)
const local_strage_key = 'visited'
const visited_state = (key) => {
  if (!localStorage.getItem(key)) {
    localStorage.setItem(key, true)
    return false
  }
  return true
}

const visited = visited_state(local_strage_key)
```


## chrome で確認する

![[Pasted image 20210831142217.png]]


## local Strage はセキュアではない!!

https://techracho.bpsinc.jp/hachi8833/2019_10_09/80851

コンソールからでも簡単にアクセス出来るので、悪いスクリプトがかっさらえるようだ。

![[Pasted image 20210831143907.png]]
