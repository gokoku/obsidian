#vscode 

---
2021-12-02

## スニペットファイルのアクセス

![[Pasted image 20211202175612.png]]

この後、言語を指定する。

![[Pasted image 20211202175722.png]]

prefix と body を指定する。

```json

{
	"Debug log": {
		"prefix": "debug",
		"body": [ "Debug.Log(\"$1\")"],
	},
	"Debug Format": {
		"prefix": "debugFormat",
		"body": ["Debug.LogFormat(\"$1\",$2)"]
	}
}
```


deb と入力するとサジェストがリストアップされる。

![[Pasted image 20211202180708.png]]

