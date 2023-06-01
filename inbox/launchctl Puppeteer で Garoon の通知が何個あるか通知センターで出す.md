---
type: note
---

#js/puppeteer 

---
2022-06-02  16:21

# launchctl Puppeteer で Garoon の通知が何個あるか通知センターで出す

`~/bin/ruby/garoon_notice_number.rb`

```ruby
require 'base64'
require 'faraday'
require 'json'
require 'pry'
require 'dotenv'

Dotenv.load

cybozu = Base64.encode64("#{ENV['GAROON_ID']}:#{ENV['GAROON_PW']}")

conn = Faraday.new( url: ENV['GAROON_URL']) do |f|
  f.request :url_encoded
  #f.response :logger
  f.adapter Faraday.default_adapter
  f.request(:authorization, :basic, ENV['GAROON_BASIC_ID'], ENV['GAROON_BASIC_PW'])
  f.headers['X-Cybozu-Authorization'] = cybozu
end

res = conn.get('/g/api/v1/notification/items')

json = JSON.load(res.body)
n = json['items'].size
print n
```


この数字を通知で出したい。

terminal から通知を出せる terminal-notifier をインストールする。

```shell
$ brew install terminal-notifier
```

`~/bin/garron_notice_number`

```shell
#!/bin/bash

RUBY=/Users/george/.asdf/shims/ruby

cd ~/bin/ruby

NUM=$(${RUBY} garoon_notice_number.rb)

if [ "${NUM}" -ge 8 ]
then
    echo  "ガルーンの通知が${NUM}個あります" | /usr/local/bin/terminal-notifier -sound default -open 'file:\
  ///Users/george/bin/notices'
fi

echo "$(date) : ガルーンの通知が${NUM}個あります"
```

```shell
$ chmod +x ~/bin/garron_notice_number

$ garron_notice_number
```

通知許可しますか通知が出るので、

![[Pasted image 20220602162829.png | 400]]

許可にすると

```shell
$ garron_notice_number 
```

![[Pasted image 20220602162940.png]]

突くと、notices 自前スクリプトでガルーンの通知をターミナルで表示して、y でブラウザを puppeteer が通知を全部開く。

この通知スクリプトを launchctl で実行したい。

10 分おきに起動させたい。

## Launchctl のインターバル実行

`~/Library/LaunchAgents/garoon.notice.number.plist`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>garoon.notice.number</string>
  <key>ProgramArguments</key>
  <array>
    <string>/Users/george/bin/garoon_notice_number</string>
  </array>
  <key>StartInterval</key>
  <integer>60</integer>
  <key>StandardErrorPath</key>
  <string>/tmp/notices.error</string>
  <key>StandardOutPath</key>
  <string>/tmp/notices.log</string>
</dict>
</plist>
```

60秒で試してみる。

```shell
$ launchctl load ~/Library/LaunchAgents/garoon.notice.number.plist
```

上手く通知が飛んでくる。
なので、標準出力と標準エラー出力を/dev/null にして、600 sec にする。

```shell
$ launchctl unload ~/Library/LaunchAgents/garoon.notice.number.plist
$ launchctl load ~/Library/LaunchAgents/garoon.notice.number.plist
```

