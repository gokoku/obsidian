---
type: note
---

#js

---
2022-06-30  18:00

# js  ローカルのテキストファイルを読み出す


<div class="rich-link-card-container"><a class="rich-link-card" href="https://stackoverflow.com/questions/14446447/how-to-read-a-local-text-file" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://cdn.sstatic.net/Sites/stackoverflow/Img/apple-touch-icon@2.png?v=73d79a89bded')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">How to read a local text file?</h1>
		<p class="rich-link-card-description">
		I’m trying to write a simple text file reader by creating a function that takes in the file’s path and converts each line of text into a char array, but it’s not working.

function readTextFile() {...
		</p>
		<p class="rich-link-href">
		https://stackoverflow.com/questions/14446447/how-to-read-a-local-text-file
		</p>
	</div>
</a></div>



```js
const logFileText = async file => {
  const response = await fetch(file)
  const text = await response.text()
  console.log(text)
}

logFileText('path/file.txt')
```

↑この人のこれが凄い!!
これが知りたかった。


これを元にして読み出したらブラウザに表示したいなら、promise で返ってくるので、then で処理すると上手くいく!! 

```js
const fileText = async file => {
  const response = await fetch(file)
  const text = await response.text()
  return(text)
}

fileText('path/file.txt').then((res) => {
  const text = res.text()
  .....
})
```
