# 写真网站爬取图片

[要爬取的写真图片网站](http://binjix.top/forum.php)

## 准备

爬取以下三个有内容的人物列表
1. [日本萝莉写真图](http://binjix.top/forum.php?mod=forumdisplay&fid=41)
2. [萝莉视频写真](http://binjix.top/forum.php?mod=forumdisplay&fid=2)
3. [偶像视频写真](http://binjix.top/forum.php?mod=forumdisplay&fid=36)

点进单个人物的链接，每个人物有若干张写真图片

为爬取下来图片的存储设计的目录结构示意为
```txt
├─日本萝莉写真图
│  ├─Ayane Yamasaka - 山坂あやね
│  ├─Horikawa Suzu - 堀川すず
│  ├─Saki Tojo - 塔上さき
│  └─Mao Imaizumi - 今泉まお
│      ├─214444q2arzhwjbzszbth8.jpg
│      ├─214653oghhlbdph9b7hl9z.jpg
│      └─214808hr6vcb2v7cnwoflt.jpg
├─萝莉视频写真
└─偶像视频写真
```
其中：
1. 一级文件夹名为 **人物列表的标题**
![](resources/2023-07-17-16-38-24.png)
2. 二级文件夹名为 **提取出的英文名 - 提取出的日文名**
![](resources/2023-07-17-16-44-03.png)
3. 三级图片文件名为 **图片链接里的文件名**
![](resources/2023-07-17-16-24-32.png)

下面用xpath_helper插件调试和获取想要的内容

### 一级文件夹名的获取

[日本萝莉写真图页面](http://binjix.top/forum.php?mod=forumdisplay&fid=41)

```py
//div[@class="boardnav"]//h1/a/text()
```

![](resources/2023-07-17-16-58-12.png)

### 单个人物详情页面的链接的获取

[日本萝莉写真图页面](http://binjix.top/forum.php?mod=forumdisplay&fid=41)

```py
//ul[@id="waterfall"]/li/div[@class="c cl"]/a/@href
```

![](resources/2023-07-17-22-26-29.png)
注意：拿到的 href 还要加上前缀
```url
http://binjix.top/
```

### 二级文件夹名的获取

[今泉まお详情页面](http://binjix.top/forum.php?mod=viewthread&tid=77&extra=page%3D1)

```py
//div[@class="t_fsz"]//font/font[contains(string(),"英文名") or contains(string(),"日文名")]/text()
```

[XPath匹配含有指定文本的标签教程](https://blog.csdn.net/butterfly5211314/article/details/82755600)

![](resources/2023-07-17-17-51-45.png)

二级文件夹名只需要提取出英文名和日文名，再拼接

### 三级图片文件名和下载链接的获取

[今泉まお详情页面](http://binjix.top/forum.php?mod=viewthread&tid=77&extra=page%3D1)

```py
//td[@class="t_f"]/ignore_js_op/img/@src
```

![](resources/2023-07-17-17-41-53.png)
注意：拿到的 src 还要加上前缀
```url
http://binjix.top/
```

三级图片文件名只需要截取 src 中最后一个 / 符号后面的内容

## 初始化项目

用scrapy爬虫框架创建爬虫

在settings.py文件中加入UA，并选择不遵守robots协议
```py
# Crawl responsibly by identifying yourself (and your website) on the user-agent
USER_AGENT = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36"
# Obey robots.txt rules
# ROBOTSTXT_OBEY = True
```

scrapy_portrait/scrapy_portrait/spiders/portrait.py文件的内容为
```py
import scrapy

class PortraitSpider(scrapy.Spider):
    name = "portrait"
    allowed_domains = ["binjix.top"]
    start_urls = ["http://binjix.top/forum.php?mod=forumdisplay&fid=41",
                  "http://binjix.top/forum.php?mod=forumdisplay&fid=2",
                  "http://binjix.top/forum.php?mod=forumdisplay&fid=36"]
    # 用 href 和 src 访问时需要添加的前缀
    url_prefix = "http://binjix.top/"

    def parse(self, response):
        print('写真爬虫启动成功')
        # 获取一级文件夹名
        page_title = response.xpath('//div[@class="boardnav"]//h1/a/text()').extract_first()
        print(page_title)
        # 获取单个人物详情页面的链接列表
        href_list = response.xpath('//ul[@id="waterfall"]/li/div[@class="c cl"]/a/@href').extract()
        for each_href in href_list:
            href_str = self.url_prefix + each_href
            print(href_str)
```

运行结果如下图
![](resources/2023-07-17-22-29-36.png)
现在已经能够正确爬取父级页面，并且可以获得子级页面的链接

## 爬取子级页面

portrait.py文件的内容修改为
```py

```



