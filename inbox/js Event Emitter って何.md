---
type: note
---

#js

---
2022-11-07  09:55

# js Event Emitter って何


<div class="rich-link-card-container"><a class="rich-link-card" href="https://javascript.plainenglish.io/as-a-front-end-engineer-the-magic-behind-event-emitter-in-javascript-that-you-should-know-about-d30a62bc4bce" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://miro.medium.com/max/1200/0*rQIHoUIFDXpSuLbI')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">As a Front-End Engineer: The Magic Behind “Event Emitter” in JavaScript That You Should Know About</h1>
		<p class="rich-link-card-description">
		5 lines of code to implement Event Emitter.
		</p>
		<p class="rich-link-href">
		https://javascript.plainenglish.io/as-a-front-end-engineer-the-magic-behind-event-emitter-in-javascript-that-you-should-know-about-d30a62bc4bce
		</p>
	</div>
</a></div>



## CustomEvent というのがある


<div class="rich-link-card-container"><a class="rich-link-card" href="https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent/CustomEvent" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://developer.mozilla.org/mdn-social-share.cd6c4a5a.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">CustomEvent() - Web APIs | MDN</h1>
		<p class="rich-link-card-description">
		The CustomEvent() constructor creates a new CustomEvent object.
		</p>
		<p class="rich-link-href">
		https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent/CustomEvent
		</p>
	</div>
</a></div>
ここにある例
```js
const catFound = new CustomEvent('animalfound', {
  detail: {
    name: 'cat'
  }
})

const dogFound = new CustomEvent('animalfound', {
  detail: {
    name: 'dog'
  }
})

window.addEventListener('animalfound', (e) => console.log(e.detail.name))
window.dispatchEvent(catFound)
window.dispatchEvent(dogFound)
```

>cat
>dog

# 記事のコード

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
<script src="script.js"></script>
</body>
</html>
```

```js
class EventEmitter {
  constructor() {
	  this.events = {}
  }
  on(evt, callback, ctx) {
	  if(!this.events[ evt ]) {
		  htis.events[ evt ] = []
	  }
	  this.events[ evt ].push(callback)
	  return this
  }
  emit(evt, ...payload) {
	  const callbacks = this.events[ evt ]
	  if(callbacs) {
		  callbacks.forEach((cb) => cb.apply(this.payload))
	  }
  }
  off(evt, callback) {
	  if(typeof evt === 'undefined') {
		  delete this.events
	  } else if(typeof evt === 'string') {
		  if(typeof callback === 'function') {
			  this.events[ evt ] = this.events[ evt ].filter((cb) => cb !== callback)
		  } else {
			  delete this.events[ evt ]
		  }
	  }
	  return this
  }
}

const e1 = new EventEmitter()
const e1Callback = (name) => {
	console.log(name, 'e1Callback')
}
const e2Callback = (name, sex) => {
    console.log(name, 'e2Callback')
}

e1.on('evt1', e1Callback)
e2.on('evt2', e2Callback)

e1.emit('evt1', 'fatfish')
e1.emit('evt2', 'medium')

e1.off('evt1', e1CAllback)
e1.emit('evt1', 'fatfish')
e1.emit('ent2', 'medium')
```

fatfish e1Callback
script.js:40 medium e2Callback

script.js:40 medium e2Callback


### apply のところを解る

```js
const number = [5,6,2,3,7]
const max = Math.max.apply(null, numbers)
console.log(max) // 7
```

func.apply( thisArg, [ argsArray] )

thisArg はメソッドで参照されないこともあるが、必須。

```js
callbasks.forEach((cb) => cb.apply(this. payload))

// これはこれと同じ感じ
((name) => console.log(name, 'e1Callback')).apply(this, ["evt1"])

// 因みにスプレッドオペレータで受けると、配列として入る
const a = (...payload) => console.log(payload)
a('abd')
//=>  ['abc']
```

