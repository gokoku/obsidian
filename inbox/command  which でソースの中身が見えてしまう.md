#command 

---
2022-01-27  12:32

# command  which でソースの中身が見えてしまう

```shell
$ which asdf
asdf () {
	local command
	command="$1"
	if [ "$#" -gt 0 ]
	then
		shift...
```

困った。パスが知りたいのに、function の時はこうなる。

```shell
$ which -p asdf
/Users/george/.asdf/bin/asdf
```

-p をつけるとOK。

which は whence -f を評価するらしい。??

<div class="rich-link-card-container"><a class="rich-link-card" href="https://stackoverflow.com/questions/7719757/zsh-which-rvm-or-which-gem-returns-the-function-contents-instead-of-the-path" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://cdn.sstatic.net/Sites/stackoverflow/Img/apple-touch-icon@2.png?v=73d79a89bded')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Zsh `which rvm` or `which gem` returns the function contents instead of the path</h1>
		<p class="rich-link-card-description">
		I've never had this problem before with my other machines but for some reason in ZSH whenever I type

which gem
or

which rvm
I get the function contents:

gem () {
local result
command gem "$@"
...
		</p>
		<p class="rich-link-href">
		https://stackoverflow.com/questions/7719757/zsh-which-rvm-or-which-gem-returns-the-function-contents-instead-of-the-path
		</p>
	</div>
</a></div>


```shell
$ whence -p asdf
これでもオッケー、てゆーかこれをラップしてるのが which なのか?
```

