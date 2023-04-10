---
type: note
---

#ai 

---
2023-03-16  13:58

# VoiceVox を使ってみる

https://voicevox.hiroshiba.jp/

![[Pasted image 20230316140003.png]]

商用・非商用問わずに無料なんてすごすぎ!!

![[Pasted image 20230316140045.png]]

オールプラットフォーム!!

mac で dmg

![[Pasted image 20230316144209.png]]

なんとアクセントイントネーション長さを変更出来る! けど、拘っていじると余計変になる。

# サーバとして起動されているらしい


<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/Haruyama_Dev/items/5b8ac0260cdfeff47121" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9VW5pdHklRTMlODElQTdWT0lDRVZPWCVFMyU4MiU5MlJFU1QtQVBJJUU3JUI1JThDJUU3JTk0JUIxJUUzJTgxJUE3JUU1JTg4JUE5JUU3JTk0JUE4JUUzJTgxJTk5JUUzJTgyJThCJUU2JTk2JUI5JUU2JUIzJTk1JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz0zYTUzZTQwNzA3ZTQ1M2IxNjgzMTNhMzZlMDRkYjFmYQ&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwSGFydXlhbWFfRGV2JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz0xMzE3NTBmNmUwM2I5ODA3MDVhMmY1N2JkMDFlOGNjYw&blend-x=142&blend-y=491&blend-mode=normal&s=d1d13478fddb86d51a34c842ffe6e2bf')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">UnityでVOICEVOXをREST-API経由で利用する方法 - Qiita</h1>
		<p class="rich-link-card-description">
		概要 UnityでREST-API経由でVOICEVOXを使用する方法の紹介 VOICEVOXのREST-APIクライアントクラスの作成 実際にUnityで喋らせるためのテストスクリプトの作成 手順  【１】VOI...
		</p>
		<p class="rich-link-href">
		https://qiita.com/Haruyama_Dev/items/5b8ac0260cdfeff47121
		</p>
	</div>
</a></div>
http://localhost:50021

![[Pasted image 20230316144733.png]]

https://snuow.com/blog/%E3%80%90python%E3%80%91voicevox%E3%82%92python%E3%81%8B%E3%82%89%E4%BD%BF%E7%94%A8%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95/

これを打ち込むとしゃべる!!

```python
import requests, json
import io
import wave
import pyaudio
import time

class Voicevox:
  def __init__(self, host="127.0.0.1", port=50021):
    self.host = host
    self.port = port

  def speak(self, text=None, speaker=47):
    params = (
      ("text", text),
      ("speaker", speaker)
    )

    init_q = requests.post(
      f"http://{self.host}:{self.port}/audio_query",
      params=params
    )
    res = requests.post(
      f"http://{self.host}:{self.port}/synthesis",
      headers={"content-Type": "application/json"},
      params=params,
      data=json.dumps(init_q.json())
    )

    audio = io.BytesIO(res.content)

    with wave.open(audio, 'rb') as f:
      p = pyaudio.PyAudio()

      def _callback(in_data, frame_count, time_info, status):
        data = f.readframes(frame_count)
        return (data, pyaudio.paContinue)

      stream = p.open(format=p.get_format_from_width(width=f.getsampwidth()),
                      channels=f.getnchannels(),
                      rate=f.getframerate(),
                      output=True,
                      stream_callback=_callback)
      # Voice 再生
      stream.start_stream()
      while stream.is_active():
        time.sleep(0.1)

      stream.stop_stream()
      stream.close()
      p.terminate()

def main():
  vv = Voicevox()
  vv.speak(text='こんにちは')

if __name__ == "__main__":
  main()
```

```shell
$ pip install requests
$ pip install pyaudio

$ python voicevox.py
```

VScode　の Debug で run 出来る。

![[Pasted image 20230316153116.png]]

run and Debug クリックすると、venvを立ち上げてターミナルが起動して実行する。便利、知らなかった。
