#raspberry_pi #node-red 

---
2021-06-14

# Node-RED 画像のパス設定

URL が /images でアクセスできるようにパスを設定出来る。

./node-red/
```js
	httpStatic: '/home/pi/MEDIA/',
```

/media から /home に変更して、USBメモリ一つで済むようにした。

けど、uibuilder ではなんか効かなかったので、ソフトリンクを張った。

```shell
~/.node-red/projects/tono-ai-node-red/uibuilder/ui/src/audio
~/.node-red/projects/tono-ai-node-red/uibuilder/ui/src/images
```

audio --> /home/pi/MEDIA/audio/
images --> /home/pi/MEDIA/images/

