#python 



気象庁からの防災情報




```python
import urllib3
import json

http = urllib3.PoolManager()
res = http.request('GET', 'https://www.jma.go.jp/bosai/forecast/data/overview_forecast/030000.json')

data = json.loads(res.data)


sentence = "天気です。{0}".format(data['headlineText'])

with open('/home/pi/AI/weather/info.txt', 'w') as f:
    print(sentence)
    f.write(sentence)
```

盛岡地方気象台の岩手県の天気情報

030000

大雑把に県だけ JSON API になってるようだ。