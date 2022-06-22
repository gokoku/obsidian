#obsidian 

---
2022-04-12  16:35

# obsidian obsidian-tasks プラグインをガッツリ導入

チェックボックスを大々的に使いたい。
行末にハッシュタグをつけて、アグリゲートしたい。

その時にチェックボックスを付けて拾わないとかコントロールしたい。

アグリゲートしたページが markdown プレビューでなるべく可愛く表示したい。


<div class="rich-link-card-container"><a class="rich-link-card" href="https://schemar.github.io/obsidian-tasks/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://schemar.github.io/obsidian-tasks/favicon.ico')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Introduction</h1>
		<p class="rich-link-card-description">
		Task management for Obsidian
		</p>
		<p class="rich-link-href">
		https://schemar.github.io/obsidian-tasks/
		</p>
	</div>
</a></div>

### 全タスクのアグリゲート
![[Pasted image 20220412170133.png]]

### done のアグリゲート
![[Pasted image 20220412164825.png]]

```tasks
done
```


### not done のアグリゲート

````
```tasks
not done
short mode
```
````

short mode の見え方がいい。


```tasks
not done
short mode
```


### 日付指定

````

```tasks
done after 2021-01-01
```
````

終わった後に日付付けてくれるから、終わったやつでしか指定出来ないのか?

```tasks
done after 2021-01-01
```





タグでフィルタリング出来ないかな。


### 数指定の場合

````
```tasks
short mode
limit 3
```
````

```tasks
short mode
limit 3
```

