#js

---
2022-03-31  17:15

# js  Async Await の仕方

https://tcd-theme.com/2021/09/javascript-asyncawait.html?gclid=Cj0KCQjw_4-SBhCgARIsAAlegrU6KdwQsCysV1HHLYb_-3PdPqpsuDTxmYC6J5TywJ4qPzPdP82EoBIaAurzEALw_wcB


<div class="rich-link-card-container"><a class="rich-link-card" href="https://tcd-theme.com/2021/09/javascript-asyncawait.html?gclid=Cj0KCQjw_4-SBhCgARIsAAlegrU6KdwQsCysV1HHLYb_-3PdPqpsuDTxmYC6J5TywJ4qPzPdP82EoBIaAurzEALw_wcB" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://tcd-theme.com/wp-content/uploads/2021/09/javascript-asyncawait.jpg')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">【JavaScriptの応用】Promiseよりも分かりやすい？Async/Awaitを使った非同期処理の方法</h1>
		<p class="rich-link-card-description">
		JavaScriptで非同期処理を行うには、コールバック関数やPromiseを使う方法があります。それらに加え、ES7からは「Async/Await」と呼ばれる非同期処理を同期処理的に書く手法が導入されました。
		</p>
		<p class="rich-link-href">
		https://tcd-theme.com/2021/09/javascript-asyncawait.html?gclid=Cj0KCQjw_4-SBhCgARIsAAlegrU6KdwQsCysV1HHLYb_-3PdPqpsuDTxmYC6J5TywJ4qPzPdP82EoBIaAurzEALw_wcB
		</p>
	</div>
</a></div>

```js
const sample = async () => {
	return 123
}

sample().then(value => console.log(value))
```


```js
const promiseFunc = value => {
	return new Promise((resolve, reject) => {
		setTimeout(() => resolve(value), 1000)
	})
}

const asyncFunc = async () => {
	console.log(await promiseFunc(1))
	console.log(await promiseFunc(2))
	console.log(await promiseFunc(3))
}

asyncFunc()
1
2
3
```


