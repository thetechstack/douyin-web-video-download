# 抖音网页版视频下载

## 方法1：直接在浏览器控制台获取

在控制台输入

``` javascript
console.log('https://'+SSR_RENDER_DATA['43']['aweme']['detail']['video']['playAddr'][0]['src'])
```

## 方法2：python实现，依赖selenium

### 准备

先到[chromedriver.chromium.org](https://chromedriver.chromium.org/downloads)下载对应Chrome版本的chromedriver.exe

### 完整代码


``` python
from selenium import webdriver
from lxml import etree
from urllib.parse import unquote
import json

class Browser:
    
    def __init__(self, driver_path):
        self.browser = webdriver.Chrome(driver_path)

    def get_douyin_video_urls(self, douyin_web_url):
        self.browser.get(douyin_web_url)
        html = etree.HTML(self.browser.page_source)
        render_data = html.xpath('//script[@id="RENDER_DATA"]')[0].text
        meta = json.loads(unquote(render_data))
        play_addrs = meta['43']['aweme']['detail']['video']['playAddr']
        play_addrs = ['https:'+play_addr['src'] for play_addr in play_addrs]
        return play_addrs
    
    def close(self):
        self.browser.close()
```

```python
browser = Browser('d:/chromedriver_win32/chromedriver.exe')
video_urls = browser.get_douyin_video_urls('https://www.douyin.com/video/6963452081848028430')
print(video_urls)

['https://v26-web.douyinvod.com/7d2fd9e44ddbdc3e12c803b0068313de/62528682/video/tos/cn/tos-cn-ve-15/709239dfe39e461f96dfbcdca02c1ce7/?a=6383&br=1031&bt=1031&cd=0%7C0%7C0%7C0&ch=26&cr=0&cs=0&cv=1&dr=0&ds=3&er=&ft=5q_lc5mmnPD12NeeK0.-UxqLFuYKc3wv25y3&l=021649571926030fdbddc0100fff0030ac3339c00000068c19f69&lr=all&mime_type=video_mp4&net=0&pl=0&qs=0&rc=amQ5ZmV2eDxsNTMzNGkzM0ApMzM4M2dmaTw2N2k5ZjY3aWcxc2E1ZWYzXmVgLS1kLTBzc2IxNTIzMzUvLWMuY2FfYy46Yw%3D%3D&vl=&vr=',
 'https://v3-web.douyinvod.com/8cb551f54b9f161de2b37660d4d4e232/62528682/video/tos/cn/tos-cn-ve-15/709239dfe39e461f96dfbcdca02c1ce7/?a=6383&br=1031&bt=1031&cd=0%7C0%7C0%7C0&ch=26&cr=0&cs=0&cv=1&dr=0&ds=3&er=&ft=5q_lc5mmnPD12NeeK0.-UxqLFuYKc3wv25y3&l=021649571926030fdbddc0100fff0030ac3339c00000068c19f69&lr=all&mime_type=video_mp4&net=0&pl=0&qs=0&rc=amQ5ZmV2eDxsNTMzNGkzM0ApMzM4M2dmaTw2N2k5ZjY3aWcxc2E1ZWYzXmVgLS1kLTBzc2IxNTIzMzUvLWMuY2FfYy46Yw%3D%3D&vl=&vr=']
```
