#git

---
2022-03-21  23:22

# PUSH エラー src refspec squash matches more than one

```shell
$ git branch -c squash
$ git tag squash

$ git push -u origin squash

error: src refspec squash matches more than one
```

error: src refspec squash matches more than one

は tag と branch が同じ名前の時に発生するとのこと。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://blanktar.jp/blog/2020/06/git-branch-matches-more-than-one" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://blanktar.jp/img/blanktar-logo@512.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">gitの「src refspec refs/heads/master matches more than one」ってエラーの直し方</h1>
		<p class="rich-link-card-description">
		GitHub Actionsで色々試行錯誤していたところ、突然「src refspec refs/heads/master matches more than one」というエラーが出て`git push`出来なくなってしまいました。この原因と、対処方法についての記事です。
		</p>
		<p class="rich-link-href">
		https://blanktar.jp/blog/2020/06/git-branch-matches-more-than-one
		</p>
	</div>
</a></div>

tag消したら push 出来た
```shell
$ git tag -d squash
```

## tag 名を変えて push

```shell
$ git tag squash-0.1

$ git push origin squash-0.1
```

![[Pasted image 20220321233032.png]]

一斉に全部 tag を送るには、

```shdll
$ git push origin --tags
```

