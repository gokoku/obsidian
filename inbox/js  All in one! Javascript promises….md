#js 

---
2022-02-25  08:42

# js  All in one! Javascript promises…


<div class="rich-link-card-container"><a class="rich-link-card" href="https://medium.com/dhiwise/all-in-one-javascript-promises-77b5bd7c7c61" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">All in one! Javascript promises…</h1>
		<p class="rich-link-card-description">
		Okay! So before starting we’ll make a promise to each other 😅 My Promise: I’ll try to explain Javascript Promises, in the best way…
		</p>
		<p class="rich-link-href">
		https://medium.com/dhiwise/all-in-one-javascript-promises-77b5bd7c7c61
		</p>
	</div>
</a></div>



コードstyle には 2つある。

- Old-style callbacks
- Newer promise-style code

これ基本形。
```js
let promise = new Promise((resolve, reject)=>{

})
```
resolve(value)--if the job is finshed successfully, with result value.
reject(error)----if an error has occurred, an error is the error object.


## Promise Chaining

```js
chooseProduct()
.then(up => selectProduct(up))
.then(cart=> addToCart(cart))
.then(buy=> buyProduct(buy))
.catch(failureCallback);
```

```js
const readData = async() => {
 try{
     const response = await fetch(URL);
     console.log(response);
  }
 catch(err){
    console.log(err);
  }
}

```


Promise のスタティックメソッドはこれらがある。

- Promise.all(iterable)
- Promise.rase(iterable)
- Promise.reject(reaseon)
- Promise.resolve(value)



### Promise.all(iterable)
```js
p1 = Promise.resolve(50)
p2 = 200      <---------------- !!
p3 = new Promise((resolve, reject) => setTimeOut(resolve, 100, 'geek'))

Promise.all([p1,p2,p3]).then((values) => console.log(values))

//output => [ 50, 200, 'geek']

```

上の input で reject されたら reject になる。

### Promise.race(iterable)

```js
const p1 = new Promise((resolve, reject) => {  
    setTimeout(() => {  
        console.log('The first promise has resolved');  
        resolve(10);  
    }, 1 * 1000);});
const p2 = new Promise((resolve, reject) => {  
    setTimeout(() => {  
        console.log('The second promise has resolved');  
        resolve(20);  
    }, 2 * 1000);  
});
Promise.race([p1, p2])  
    .then(value => console.log(`Resolved: ${value}`))  
    .catch(reason => console.log(`Rejected: ${reason}`));

//output 
The first promise has resolved  
Resolved: 10  
The second promise has resolved
```

Promise.race でのresolve の値は一番早いやつ。
因みに Promise.all でやると [10,20] となる。






Promise のインスタンスメソッドはこれらがある。

- Promise.prototype.then()
- Promise.prototype.catch()
- Promise.prototype.finally()


