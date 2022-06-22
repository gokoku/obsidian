#js 


[https://blog.decipher.dev/how-you-should-not-write-code-or-javascript](https://blog.decipher.dev/how-you-should-not-write-code-or-javascript)

古いスタイルでは private vaiables が使えなかった?

アンダースコアつけたりして、

```jsx
class Person {
   constructor(nm) {
		this.setName = function(name) {
			nm = name
		}
		this.getName = function() {
			return nm
		}
	}
}

const person = new Person("deepak");
console.log(person.getName()); // "deepak"
person.setName("kapeed");
console.log(person.getName()); // "kapeed"
```

---

clojure 版では

```jsx
function PersonF(nm) {
	this.setName = function (name) {
		nm = name
	}
	this.getName = function() {
		return nm
	}
}
const personf = new PersonF("deepak");
console.log(personf.getName()); // "deepak"
personf.setName("kapeed");
console.log(personf.getName()); // "kapeed"
```

しかし、Symbol を使うと、

```jsx
const property = Symbol()
class Something {
	constructor() {
		this[properth] = "test"
	}
}

var instance = new Something()
console.log(instance["property"]    // undefined
condole.log(instance)               // Something { [Symbol()]: 'test'}
```

よくわからん。

外から使えないということかな?
では、中で使うにはどうやるのかな?

```jsx
const property = Symbol()
class Something {
	constructor() {
		this[property] = 'test'
	}
	name () {
		return this[property]
	}
}

var instatnce = new Something()
console.log(instance.name())   //test
```

ピンとこない。　List, Ruby のシンボルに当たるような感じの唯一無二のリテラルって感じなのだが、

---

値がないのをnull で表現しているとよくない。わかってるがしかし。
undefined とも false とも違う。うん。
でも、値がないのを安全に表現したいとき、唯一無二のSymbol がいいとのこと。

```jsx
const NO_NAME = Symbol()
class BetterPerson {
  constructor(name) {
    this.name = name || NO_NAME
  }

  hasName() {
    return this.name != NO_NAME
  }
}

deepak = new BetterPerson()
console.log(deepak.hasName())      // false

deepak = new BetterPerson(0)
console.log(deepak.hasName())      // false

deepak = new BetterPerson('')
console.log(deepak.hasName())      // false

deepak = new BetterPerson(deepak)
console.log(deepak.hasName())      // true
```

