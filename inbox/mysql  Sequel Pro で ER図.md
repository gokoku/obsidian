#mysql

---
2022-01-13

# Sequel Pro でER図

https://zenn.dev/katoaki/articles/17d737090c34be

エクスポートで dot ファイルがある!!!

![[Pasted image 20220113153859.png]]

```shell
$ brew install graphviz
```

Ubuntu は

```shell
$ sudo apt install graphviz
```

### Graphviz

```shell
$ dot -Tsvg yohira_dev.dot > yohira_dev.
```

