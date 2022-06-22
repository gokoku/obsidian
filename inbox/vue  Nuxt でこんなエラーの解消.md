#js/vue 

```shell
Component names should conform to valid custom element name in html5 specification.

```

とでたら、そのコンポーネントファイルの中に name 属性を付けると何故か解決する。

```bash
export default {
	name: "hoge",
}
```