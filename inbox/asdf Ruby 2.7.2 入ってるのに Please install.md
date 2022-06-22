---
type: note
---

#asdf #ruby

---
2022-05-17  14:21

# asdf Ruby 2.7.2 入ってるのに Please install

asdf で ruby 2.7.2 入れた。
今は 3.1.2 で動かしてる。

```shell
$ asdf install ruby 2.7.2
$ asdf local ruby 2.7.2

$ foreman start -f Profile.dev

No preset version installed for command foreman
Please install a version by running one of the following:

asdf install ruby 2.7.2

or add one of the following versions in your config file at /Users/george/Desktop/trial-rais7/.tool-versions
ruby 3.1.0
```

分からん。

これで動いた。

```shell
$ asdf exec gem install foreman
```

でも、about する。もういいや。


