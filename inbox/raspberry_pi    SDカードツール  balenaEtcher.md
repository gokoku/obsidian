#raspberry_pi 


[balenaEtcher - Flash OS images to SD cards & USB drives](https://www.balena.io/etcher/)

どうしてもバックアップからリストア出来なかった謎がこのツールでわかった(かも?)

32G と書いてても、実は微妙に違うことが判明、バックアップイメージが 31.7G だったので、31.7Gが確保出来るSDカードがリストアに成功して、31Gだったものが失敗してるようだった。

このツールは今どきで良さげで気に入りました。

![[Pasted image 20210617153211.png]]

balenaEtcher


```shell
$ brew install balenaetcher
```

/Applications/balenaetcher.app が入る。

## usage

ディスクイメージから USB ドライブに移すだけのようだ。

