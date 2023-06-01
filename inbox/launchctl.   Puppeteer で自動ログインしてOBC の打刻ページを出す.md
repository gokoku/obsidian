---
type: note
---

#js/puppeteer #command

---
2022-06-02  16:05

# launchctl.   Puppeteer で自動ログインしてOBC の打刻ページを出す
shell スクリプト

`~/bin/obc`

```shell
!/bin/bash

NODE=$HOME/.asdf/shims/node

cd ~/bin/puppeteer
$NODE obc_time_rec.js
```

`~/bin/puppeteer/obc_time_recc.js`

```js
require("dotenv").config()
const puppeteer = require("puppeteer")

const BASICID = process.env.G_BASIC_ID
const BASICPASS = process.env.G_BASIC_PASSWORD

const USERNAME = process.env.O_USERNAME
const PASSWORD = process.env.O_PASSWORD

const URL = process.env.O_URL
;(async () => {
  const browser = await puppeteer.launch({
    headless: false,
  })
  const page = await browser.newPage()
  await page.setViewport({
    width: 1200,
    height: 1000,
  })
  await page.goto(URL)
  await page.type("input#OBCID", USERNAME)
  await page.click("button#checkAuthPolisyBtn")

  await page.waitForSelector("input#Password")

  await page.type("input#Password", PASSWORD)
  await page.click("button#login")

  await page.waitForNavigation({ waitUntil: "load" })

  await Promise.all([
    page.waitForNavigation({ waitUntil: "load" }),
    page.click("a#btn00"),
  ])
})()
```

この puppeteer ディレクトリには package.json で puppeteer, dotenv がインストールされてるので、require で使える。

.env にパスワードが書いてある。

### Launchct の 定時実行設定

定時に実行するようにする。

`~/Library/LaunchAgengs/obc.timecard.plist`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>Label</key>
	<string>obc.timecard</string>
	<key>ProgramArguments</key>
	<array>
		<string>/Users/george/bin/obc</string>
	</array>
	<key>StartCalendarInterval</key>
  <dict>
    <key>Hour</key>
    <integer>18</integer>
    <key>Minute</key>
    <integer>00</integer>
  </dict>
	<key>StandardErrorPath</key>
	<string>/dev/null</string>
	<key>StandardOutPath</key>
	<string>/dev/null</string>
</dict>
</plist>
```

```shell
$ launchctl load ~/Library/LaunchAgents/obc.timecard.plist
```

