#raspberry_pi/osmc 

---
2022-04-16  20:08

# osmc  Update 失敗してる
ディスプレイに update に

手動でアップデートする。
が、オプションつけて update
```

$ sudo apt update --allow-releaseinfo-change

$ sudo apt dist-upgrade
$ sudo reboot
```

出来た。


けど、設定が全部吹っ飛んでた。

もう一回リブートしたら戻った!!

## HAT の音が出ない

![[raspberry_pi    HIFI DAC HAT を鳴らした]]

一旦 OSCM の同じ skin にして My OSMC を出して
Settings --> My OSMC -> Hardware Support --> Soundcard Overlay
にした。

allo-boss-dac-pcm512x-audio-overlay がなかったけど、
allo-boss-dac-pcm512x-audio があったのでこれを選択して、**リブート**　かけたら音が出た!!

音楽プレイヤーとして使いにくかったので skin を PELLUCID に戻した。

PELLUCID で My OSMC 見つけた。
Settings ->Program add-ons -> My OSMC --> Hardware Suppout

試しに
allo-piano-dac-pcm512x-audio にしてみた。```sudo reboot``` 鳴らなかった。

allo-boss-dac-pcm512x-audio にして ```sudo reboot``` 鳴った!!
CD はメッチャいい音に鳴るな!!






