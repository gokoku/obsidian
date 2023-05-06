---
type: note
---

#clojure #vscode 

---
2023-05-05  21:42

# clojure  VScode のフォーマッター


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/lagenorhynque/items/a5d83b4a36a1cf1cacbe" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9Q2xvanVyZSVFOSU5NiU4QiVFNyU5OSVCQSVFNyU5MiVCMCVFNSVBMiU4MyVFMyU4MSVBNyVFMyU4MSVBRSVFMyU4MyU5NSVFMyU4MiVBOSVFMyU4MyVCQyVFMyU4MyU5RSVFMyU4MyU4MyVFMyU4MiVCRiVFMyU4MyVCQ2NsanN0eWxlJUU4JUE4JUFEJUU1JUFFJTlBJUUzJTgxJUJFJUUzJTgxJUE4JUUzJTgyJTgxJTNBJTIwU3BhY2VtYWNzJTJDJTIwSW50ZWxsaUolMjBJREVBJTIwJTI4Q3Vyc2l2ZSUyOSUyQyUyMFZTJTIwQ29kZSUyMCUyOENhbHZhJTI5JTJDJTIwVmltJTIwJUUyJTgwJUE2JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz1hYTFjZmY5NTUzOTdhZmE1YzQ5YmJiN2ZiNWYxNmMzNQ&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwbGFnZW5vcmh5bnF1ZSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9MGQyMTllN2ZiMTQyZGUyMDVlYzc0Yzc4ZGRjNDIzMDE&blend-x=142&blend-y=491&blend-mode=normal&s=66986903218d0b2f18dc387e90121fdb')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Clojure開発環境でのフォーマッターcljstyle設定まとめ: Spacemacs, IntelliJ IDEA (Cursive), VS Code (Calva), Vim (vim-iced) - Qiita</h1>
		<p class="rich-link-card-description">
		お好みのエディタ/IDEにClojure開発環境を用意したら、ぜひファイル保存時に自動実行できるフォーマッターも設定しましょう。  他言語におけるPrettierなどと同様に、ファイルを保存するたびにフォーマットできると非常に快適です...
		</p>
		<p class="rich-link-href">
		https://qiita.com/lagenorhynque/items/a5d83b4a36a1cf1cacbe
		</p>
	</div>
</a></div>


cljfmt というのがあって、それよりも cljstyle がいいらしい。

```shell
$ brew --cask install cljstyle

```

~/.cljstyle
```clojure
{:rules {:blank-lines {:max-consecutive 1
                       :padding-lines 1}
         :comments {:enabled? false}
         :functions {:enabled? false}
         :indentation {:indents {defspec [[:inner 0]]
                                 fdef [[:inner 0]]
                                 for-all [[:inner 0]]}
                       :list-indent 1}
         :namespaces {:indent-size 1}
         :types {:enabled? false}}}
```


## VScode

settings.json
```json
{
  ...,
  "saveAndRun": {
    "commands": [
      {
        "match": "\\.clj[cs]?$",
        "cmd": "cljstyle fix ${file}",
        "useShortcut": false,
        "silent": true
      }
    ]
  }
}
```


