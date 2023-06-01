#js/puppeteer



```js
const puppeteer = require('puppeteer')

;(async () => {
  let resultObj = {}
  let returnedResponse
  let browser
  try {
    browser = await puppeteer.launch({
      headless: false,
      args: [
        '--no-sandbox',
        '--disable-setuid-sandbox',
        '--disable-infobars',
        '--disable-features=site-per-process',
        '--window-position=0,0',
        '--disable-extensions',
        '--user-agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3312.0 Safari/537.36"',
      ],
    })

    const page = await browser.newPage()
    await page.setViewport({ width: 1366, height: 800 })
    await page.goto(
      'https://www.amazon.in/s?k=keyboard&tag=amot-21&ref=nb_sb_noss',
      { waitUtil: 'load', timeout: 10000 }
    )
    await page.waitForSelector('#h > div.s-desktop-width-max')
  } catch (e) {
    console.log('Amazon scrap error-> ', e)
    await browser.close()
  }
})()

```

args で Agent を設定するようだ。
