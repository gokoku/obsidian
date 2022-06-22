#ts #deno

---
2021-11-04

# Deno で http リクエスト

Deno インストール

```shell
$ asdf plugin-add deno
$ asdf list-all deno
$ asdf install deno 1.15.3
$ asdf global deno 1.15.3
$ deno --version
```

http.ts

```typescript
const res = await fetch("https://www.yahoo.co.jp");
const data = await res.text();
console.log(data);
```

```shell
$ deno run --allow-net ./http.ts
```