#js

---
2021-12-06

# setTimeout に Promise を使う

```js

const sleep = async (millisec) =>
  new Promise((resolve) => setTimeout(resolve, millisec))

sleep(3000).then(() => console.log("You'll see this after 3 seconds"))
```
