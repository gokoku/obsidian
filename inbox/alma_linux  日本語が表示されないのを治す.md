---
type: note
---

#alma_linux 

---
2023-05-01  17:00

# almalinux  日本語が表示されないのを治す

```
git log を日本語で書いてるけど、<E3><83><86><E3><83><B3><E3><83> となって日本語で読めません。どうしたらいいでしょうか
```

とchatGPTに聞いてみた。

```
$ local で確認
$ sudo apt install language-pack-ja
$ export LANG=ja_JP.UTF-8
```

で、os は AlmaLinuxです。と言ったら、

```
$ local
$ sudo dnf install glibc-langpack-ja
$ localectl set-locale LANG=ja_JP.UTF-8
```

で入り直したら、

```shell
0bf561c1a86f358ba3b9945c589e0db26bdd548d (HEAD -> develop) Merge branch 'develop' of https://gitlab.pmorioka.com/iwate-it/ogasta into develop
d468c19345780d32eaaa9848144bd9fdbe415119 (origin/develop) テンプレートのphotoに　CORS対策がセットされていなか
ったのを直した
760b5e312e92a31b22e6f498fe4bfd8b41f6fb5b Merge branch 'develop' of https://gitlab.pmorioka.com/iwate-it/ogasta into develop
69d9a68aa59e5bd6d1f923955fbe365440b9a933 fix: テンプレートのcrud修正
33ec147627e22dc07b15d0d81b0dee50ab21aae3 Merge branch 'develop' of https://gitlab.pmorioka.com/iwate-it/ogasta into develop
```

バッチリ!!
因みに locale では、LANG=ja_JP.UTF-8 だったけど、set-locale してみたらパッケージがないと言われたので、 dnf でインストールした。


