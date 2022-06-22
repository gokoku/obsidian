#deno

---
2021-12-13

# Deno install

https://deno.land/#installation

```shell
$ curl -fsSL https://deno.land/x/install/install.sh | sh

$ deno repl


$ deno run -q hello.ts   // -q はコンパイルメッセージを出さない
```


Mac は export とか面倒臭そうなので brew

```shell
$ brew install deno


```
VScode の extention

![[Pasted image 20220620144809.png]]f

sandbox.ts

```ts

// reading file

const decoder = new TextDecoder('utf-8')
const data = await Deno.rekadFile('readme.txt')
console.log(decoder.decode(data))

const data = await Deno.readTextFile('readme.txt')
console.log(data)

// writing files

const encoder = new TextEncoder()
const text = encoder.encode('hello again, ninjas')
await Deno.writeFile('readme.txt', text)

// renaming and removing files

await Deno.rename('readme.txt', 'deleteme.md')
await Deno.remove('deleteme.md')
```


```shell
$ deno run --allow-read --allow-write sandbox.ts
```


sandbox.ts
```ts
// fetch  api

const res = await fetch('https://swapi.dev/api/films/')
const data = await res.json()
console.log(data)
```

```shell
$ deno run --allow-net sandbox.ts
```

