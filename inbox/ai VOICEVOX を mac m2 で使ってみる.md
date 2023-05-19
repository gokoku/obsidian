---
type: note
---

#ai

---
2023-05-16  10:58

# ai VOICEVOX を mac m2 で使ってみる

https://voicevox.hiroshiba.jp/

![[Pasted image 20230516105902.png]]

起動する。

![[Pasted image 20230516110102.png]]


## Elixir で「ずんだもん」を喋らせよう

<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/t-yamanashi/items/75c40103b7e240c78288" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9RWxpeGlyJUUzJTgxJUE3JUUzJTgwJThDJUUzJTgxJTlBJUUzJTgyJTkzJUUzJTgxJUEwJUUzJTgyJTgyJUUzJTgyJTkzJUUzJTgwJThEJUUzJTgyJTkyJUU1JTk2JThCJUUzJTgyJTg5JUUzJTgxJTlCJUUzJTgyJTg4JUUzJTgxJTg2JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz02ZGI0M2MzOTQ4NGI1YmY3MjMxZGE5MWJhODFiZjI0MQ&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwdC15YW1hbmFzaGkmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTkzMjBjY2IyOGNiZjVmZjE5OTcxNmQ3YWM2MmVkNjc5&blend-x=142&blend-y=491&blend-mode=normal&s=a1a428b27d811d24ddfdb540eda63087')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">Elixirで「ずんだもん」を喋らせよう - Qiita</h1>
		<p class="rich-link-card-description">
		Elixirで「ずんだもん」を喋らせたいと一度は思ったことありますよね？ と言うことで実践  前提条件 ・OS： Ubuntu 22.04(実機) ・VOICEVOXのLinux版を予めインストール済みであること ・VOICEVOXが...
		</p>
		<p class="rich-link-href">
		https://qiita.com/t-yamanashi/items/75c40103b7e240c78288
		</p>
	</div>
</a></div>


早速これやってみたい。

```shell
$ asdf pulugin add erlang https://github.com/asdf-vm/asdf-erlang.git
$ asdf install erlang latest
$ asdf global erlang latest
$ erl

Eshell V13.2.2
1>
```

```
$ asdf plugin-add elixir https://github.com/asdf-vm/asdf-elixir.git
$ asdf install elixir latest
$ adsf global elixir latest
$ elixir --version
Elixir 1.14.4
```

```
$ mix new voi
$ cd voi
```

mix.exsに追記。
```elixir
  defp deps do
    [
      {:req, "~> 0.2.1"}
    ]
  end
```

```shell
$ mix deps.get
```

本体 lib/voi.ex
```elixir
defmodule Voi do
  @moduledoc """
  Documentation for `Voi`.
  """

  @doc """
  Hello world.

  ## Examples

      iex> Voi.hello()
      :world

  """
  def hello(text) do
    param = URI.encode_query(%{text: text, speaker: 1})
    json =
      ("http://localhost:50021/audio_query?" <> param)
      |> Req.post!("")
      |> Map.get(:body)
      |> Jason.encode!()


    headers = ["Content-Type": "application/json"]

    data =
      "http://localhost:50021/synthesis?speaker=1"
      |> Req.post!(json, headers: headers)
      |> Map.get(:body)

    File.rm("hoge.wav")
    File.write("hoge.wav", data)
    System.cmd("afplay", ["hoge.wav"])
  end
end

```

```shell
$ iex -S mix
> Voi.hello("初めまして、ずんだもんであります。")
```

喋った!!!!

## VOICEVOX の API を調べる

VOICEVOXを起動。

http://localhost:50021/docs

![[Pasted image 20230516140110.png]]



<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/A_T_B/items/1531d78944d8b796b9fa" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9Vk9JQ0VWT1glMjglRTklOUYlQjMlRTUlQTMlQjAlRTUlOTAlODglRTYlODglOTAlMjklRTMlODIlOTJSRVNULUFQSSVFMyU4MSVBNyVFNSU4OCVBOSVFNyU5NCVBOCVFMyU4MSU5OSVFMyU4MiU4QiZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9NWNlMDBmMTk2MTljNDY2OGY5MTk2ZmI3NWM0YWM2NDY&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwQV9UX0ImdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTc0YzdjNmQ3MGI4Mjg5NjJjMzE4OTFjYjhjZjhhZGY4&blend-x=142&blend-y=491&blend-mode=normal&s=820665368f208eef22e81ca707495247')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">VOICEVOX(音声合成)をREST-APIで利用する - Qiita</h1>
		<p class="rich-link-card-description">
		VOICEVOXとは 音声合成ソフトといえば実況動画などでお馴染みのVOICEROIDがありますが、あれは1万円ほどする上に外部ソフトウェアと連携するインターフェイスは持ちません。 一方で今回紹介するVOICEVOXは「高品...
		</p>
		<p class="rich-link-href">
		https://qiita.com/A_T_B/items/1531d78944d8b796b9fa
		</p>
	</div>
</a></div>

```
$ curl -s -X POST "localhost:50021/audio_query?speaker=1" \
--get --data-urlencode text@text.txt
```
text@ はdataを urlencode変換をするファイルパスのプレフィックスとのこと。
--get は GET パラメータに指定するとのこと。


以下はいくつかの例です：

-   `text@text.txt`：カレントディレクトリにある`text.txt`ファイルを指定します。
-   `text@/path/to/text.txt`：`/path/to/`ディレクトリにある`text.txt`ファイルを指定します。
-   `text@../folder/text.txt`：カレントディレクトリの親ディレクトリ内の`folder`ディレクトリにある`text.txt`ファイルを指定します。


```shell
$ curl -s -H "Content-Type: application/json" -X POST \
-d @query.json "localhost:50021/synthesis?speaker=1" > audio.wav
```

-d の @ はそのままデータファイルを使うプレフィックスとのこと。


## Livebookでやってみる

```elixir
Mix.install([
  {:req, "~> 0.2.1"}
])
```

```elixir
speaker = 1
text = "こんにちはなのだ"

param = URI.encode_query(%{text: text, speaker: speaker})
json =
  ("http://localhost:50021/audio_query?" <> param)
  |> Req.post!("")
  |> Map.get(:body)
  |> Jason.encode!(pretty: true)


headers = ["Content-Type": "application/json"]
data =
  "http://localhost:50021/synthesis?speaker=#{speaker}"
  |> Req.post!(json, headers: headers)
  |> Map.get(:body)
File.rm("hoge.wav")
File.write("hoge.wav", data)
System.cmd("afplay", ["hoge.wav"])
```





## Ruby で同じことをやってみる

```ruby
require 'net/http'
require 'json'
require 'uri'

speaker_no = 1
url = URI.parse('http://localhost:50021/audio_queyr')
http = Net::HTTP.new(url.host, url.port)

params = {
  text: "こんにちはなのだよ、諸君!",
  speaker: speaker_no
}
url.query = URI.encode_www_form(params)

request = Net::HTTP:Post.new(url.request_uri)
response = http.request(request)
json = resoponse.body

#--------------------------------------
url = URI.parse("http://localhost:50021/synthesis")
headers = {'Content-Type' => 'application/json'}
params = {speaker: speaker_no}

http = Net::HTTP.new(url.host, url.port)
url.query = URI.encode_www_form(params)

request = Net::HTTP::Post.new(url.request_uri, headers)
request.body = json

response = http.request(request)
data = response.body

File.open('hoge.wav', 'w') do |file|
  file.write(data)
end

system('afplay hoge.wav')
```


## なんと! React からやってみる

https://qiita.com/A_T_B/items/1531d78944d8b796b9fa

<div class="rich-link-card-container"><a class="rich-link-card" href="https://qiita.com/A_T_B/items/1531d78944d8b796b9fa" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9Vk9JQ0VWT1glMjglRTklOUYlQjMlRTUlQTMlQjAlRTUlOTAlODglRTYlODglOTAlMjklRTMlODIlOTJSRVNULUFQSSVFMyU4MSVBNyVFNSU4OCVBOSVFNyU5NCVBOCVFMyU4MSU5OSVFMyU4MiU4QiZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9NWNlMDBmMTk2MTljNDY2OGY5MTk2ZmI3NWM0YWM2NDY&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwQV9UX0ImdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTc0YzdjNmQ3MGI4Mjg5NjJjMzE4OTFjYjhjZjhhZGY4&blend-x=142&blend-y=491&blend-mode=normal&s=820665368f208eef22e81ca707495247')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">VOICEVOX(音声合成)をREST-APIで利用する - Qiita</h1>
		<p class="rich-link-card-description">
		VOICEVOXとは 音声合成ソフトといえば実況動画などでお馴染みのVOICEROIDがありますが、あれは1万円ほどする上に外部ソフトウェアと連携するインターフェイスは持ちません。 一方で今回紹介するVOICEVOXは「高品...
		</p>
		<p class="rich-link-href">
		https://qiita.com/A_T_B/items/1531d78944d8b796b9fa
		</p>
	</div>
</a></div>


```shell
$ yarn create react-app voicevox_sample --template typescript

npx でも同じことしたい。
```


```shell
$ cd voicevox_sample
$ yarn add superagent
$ yarn add @types/superagent
```

App.tsx

```typescript
import React, { ChangeEvent, useState } from 'react';
import superagent from 'superagent'
import './App.css';

const contentStyle: React.CSSProperties = {width: '80%', textAlign: 'center'}
const textareaStyle: React.CSSProperties = {width: "100%", height: 100}
const buttonStyle: React.CSSProperties = {...textareaStyle, fontSize: 30}
const audioStyle: React.CSSProperties = {...textareaStyle}

type Mora = {
  text: string
  consonant: string
  consonant_length: number
  vowel: string
  vowel_length: number
  pitch: number
}

type Query = {
  accent_phrases: {
    moras: Mora[]
    accent: number
    pause_more: Mora
  }
  speedScale: number
  pitchScale: number
  volumeScale: number
  prePhonemeLength: number
  postPhonemeLength: number
  outputSamplingRate: number
  outputStereo: boolean
  kana: string
}

const App = () => {
  const [inputText, setInputText] = useState<string>('')
  const [queryJson, setQueryJson] = useState<Query>()
  const [audioData, setAudioData] = useState<Blob>()

  const createQuery = async () => {
    const res = await superagent
      .post('http://localhost:50021/audio_query')
      .query({speaker: 1, text: inputText})

    if (!res) return
    setQueryJson(res.body as Query)
  }

  const createVoice = async () => {
    const res = await superagent
      .post('http://localhost:50021/synthesis')
      .query({ speaker: 1})
      .send(queryJson)
      .responseType('blob')

    if(!res) return
    setAudioData(res.body as Blob)
  }

  return (
    <div className='App-header'>
      <div style={contentStyle}>
        <h2>読み上げたい文章を入力</h2>
        <textarea
          style={textareaStyle}
          value={inputText}
          onChange={
            (e: ChangeEvent<HTMLTextAreaElement>) => setInputText(e.target.value)
          }
          />
      </div>

      {inputText ? (
        <div style={contentStyle}>
          <p>⇩</p>
          <button style={buttonStyle} onClick={createQuery}>クエリ作成</button>
        </div>
      ) : null}

      {
        queryJson ? (
          <div style={contentStyle}>
            <p>⇩</p>
            <h2>クエリデータから音声を合成</h2>
            <button style={buttonStyle} onClick={createVoice}>音声合成</button>
          </div>

        ) : null}

        {audioData ? (
          <div style={contentStyle}>
            <p>⇩</p>
            <h2>返却された音声ファイルを再生</h2>
            <audio
              style={audioStyle}
              controls
              src={audioData ? window.URL.createObjectURL(audioData) : undefined}>
            </audio>
          </div>
        ) : null }

    </div>
  )
}

export default App;

```
![[Pasted image 20230516172943.png]]
次々にデータが揃うと現れてくるボタン。
凄い!!
audioデータを保持して、window.URL.createObjectURL(audioData) で引っ張ってくるなんて凄いな。

データはBlobオブジェクトタイプとして受けとって useState でセットしている。
URL.createObjectURL メソッドで Blob URLを生成することで、audio の src にしているそうだ。

因みに Blob URL は新しい音声データほセットされる度に解放する必要があるとのこと。
コンポーネントがアンマウントされる時とかに

URL.revokeObjectURL をするとのこと。





## VOICEVOX ENGINE はヘッドレスサーバ?

![[Pasted image 20230516163956.png]]

