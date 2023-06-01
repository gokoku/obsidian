#mac 

---
2022-02-20  19:58

# mac  Update したら Docker が動かなくなった

brew upgrade するといいらしい。

やってみると、色々しろと言われた。

```shell
$ brew upgrade
```

結局、/Applications/Docker.app の中に入って、段階的に rm -rf で削除した。
/Library/PrivilegedHelperTools/com.docker.vmnetd
/Library/LaunchDaemons/com.docker.vmnetd.plist
を消した。

```shell
$ brew install docker --cask
```

これで動くようになった。

brew で install したから 今度からは brew upgrade で済むかも。
