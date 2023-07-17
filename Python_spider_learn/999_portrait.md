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
│      ├─Mao Imaizumi 今泉まお.jpg
│      ├─b_ppv_okinawa01_imaizumi_m01_004.jpg
│      └─b_ppv_okinawa01_imaizumi_m01_008.jpg
├─萝莉视频写真
└─偶像视频写真
```
其中：
1. 一级文件夹名为 **人物列表的标题**
![](resources/2023-07-17-16-38-24.png)
2. 二级文件夹名为 **提取出的英文名 - 提取出的日文名**
![](resources/2023-07-17-16-44-03.png)
3. 三级图片文件名为 **图片链接里的文件名**
![](resources/2023-07-18-00-47-49.png)

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

有的人物提取不出英文名或日文名，就暂时用人物详情页的标题代替

```py
//div[@id="postlist"]//span[@id="thread_subject"]/text()
```

![](resources/2023-07-17-23-58-27.png)

### 三级图片文件名和下载链接的获取

[今泉まお详情页面](http://binjix.top/forum.php?mod=viewthread&tid=77&extra=page%3D1)

下载链接的获取方法

```py
//td[@class="t_f"]/ignore_js_op/img/@src
```

![](resources/2023-07-17-17-41-53.png)
注意：拿到的 src 还要加上前缀
```url
http://binjix.top/
```

但是这在实际使用中出现了问题，因为网页中的图片使用了懒加载，加载前的src属性值放在zoomfile属性里
![](resources/2023-07-18-00-40-54.png)

所以，把 @src 修改为 @zoomfile 即可

```py
//td[@class="t_f"]/ignore_js_op/img/@zoomfile
```

图片文件名的获取方法

```py
//td[@class="t_f"]/ignore_js_op/div/div[@class="xs0"]/p[1]/strong/text()
```

![](resources/2023-07-18-00-57-04.png)

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

在portrait.py文件的PortraitSpider类中新增加一个方法parse_child()，用于爬取子级页面，portrait.py文件的内容修改为
```py
import scrapy

class PortraitSpider(scrapy.Spider):
    name = "portrait"
    allowed_domains = ["binjix.top"]
    start_urls = ["http://binjix.top/forum.php?mod=forumdisplay&fid=41",
                  "http://binjix.top/forum.php?mod=forumdisplay&fid=2",
                  "http://binjix.top/forum.php?mod=forumdisplay&fid=36"]
    # 用 href 和 src 访问时需要添加的前缀
    prefix_url = "http://binjix.top/"

    # 爬取父级页面
    def parse(self, response):
        print('写真爬虫启动成功')
        # 获取一级文件夹名
        page_title = response.xpath('//div[@class="boardnav"]//h1/a/text()').extract_first()
        print(page_title)
        # 获取单个人物详情页面的链接列表
        href_list = response.xpath('//ul[@id="waterfall"]/li/div[@class="c cl"]/a/@href').extract()
        for each_href in href_list:
            # 获取单个人物的链接
            href_str = self.prefix_url + each_href
            # 对子级页面进行访问
            yield scrapy.Request(url=href_str, callback=self.parse_child)
            
    # 爬取子级页面
    def parse_child(self, response):
        # 获取人物介绍文字列表
        info_list = response.xpath('//div[@class="t_fsz"]//font/font[contains(string(),"英文名") or contains(string(),"日文名")]/text()').extract()
        # 初始化英文名和日文名
        english_name = ''
        japanese_name = ''
        full_name = ''
        for each_info in info_list:
            # 找到介绍中的英文名和日文名
            if(english_name == '' and each_info.find('英文名：') != -1):
                # 切片，并获取：号后面的部分，并去掉前后空格
                english_name = each_info.split('：', 1)[1].strip()
            if(japanese_name == '' and each_info.find('日文名：') != -1):
                japanese_name = each_info.split('：', 1)[1].strip()
            # 如果英文名和日文名都找到就退出循环
            if(english_name != '' and japanese_name != ''):
                break
        # 如果解析出了英文名和日文名，就用 英文名 - 日文名
        if(english_name != '' and japanese_name != ''): 
            full_name = english_name + ' - ' + japanese_name
        # 如果解析不出，就用人物详情页的标题代替
        else:
            full_name = response.xpath('//div[@id="postlist"]//span[@id="thread_subject"]/text()').extract_first()
        print(full_name)  
```

运行结果如下图
![](resources/2023-07-17-23-53-13.png)
现在可以获得所有二级文件夹的命名

继续爬取子级页面中的写真图片的名字和下载链接，为接下来的下载图片做准备，portrait.py文件的内容修改为
```py
import scrapy

class PortraitSpider(scrapy.Spider):
    name = "portrait"
    allowed_domains = ["binjix.top"]
    start_urls = ["http://binjix.top/forum.php?mod=forumdisplay&fid=41",
                  "http://binjix.top/forum.php?mod=forumdisplay&fid=2",
                  "http://binjix.top/forum.php?mod=forumdisplay&fid=36"]
    # 用 href 和 src 访问时需要添加的前缀
    prefix_url = "http://binjix.top/"

    # 爬取父级页面
    def parse(self, response):
        print('写真爬虫启动成功')
        # 获取一级文件夹名
        page_title = response.xpath('//div[@class="boardnav"]//h1/a/text()').extract_first()
        print(page_title)
        # 获取单个人物详情页面的链接列表
        href_list = response.xpath('//ul[@id="waterfall"]/li/div[@class="c cl"]/a/@href').extract()
        for each_href in href_list:
            # 获取单个人物的链接
            href_str = self.prefix_url + each_href
            # 对子级页面进行访问
            yield scrapy.Request(url=href_str, callback=self.parse_child)
            
    # 爬取子级页面
    def parse_child(self, response):
        # 获取人物介绍文字列表
        info_list = response.xpath('//div[@class="t_fsz"]//font/font[contains(string(),"英文名") or contains(string(),"日文名")]/text()').extract()
        # 初始化英文名和日文名
        english_name = ''
        japanese_name = ''
        full_name = ''
        for each_info in info_list:
            # 找到介绍中的英文名和日文名
            if(english_name == '' and each_info.find('英文名：') != -1):
                # 切片，并获取：号后面的部分，并去掉前后空格
                english_name = each_info.split('：', 1)[1].strip()
            if(japanese_name == '' and each_info.find('日文名：') != -1):
                japanese_name = each_info.split('：', 1)[1].strip()
            # 如果英文名和日文名都找到就退出循环
            if(english_name != '' and japanese_name != ''):
                break
        # 如果解析出了英文名和日文名，就用 英文名 - 日文名
        if(english_name != '' and japanese_name != ''): 
            full_name = english_name + ' - ' + japanese_name
        # 如果解析不出，就用人物详情页的标题代替
        else:
            full_name = response.xpath('//div[@id="postlist"]//span[@id="thread_subject"]/text()').extract_first()
        print(full_name)
        
        # 获取写真图片的名字和下载链接，因为图片名和下载链接有一一对应的关系，所以不能分别解析
        block_list = response.xpath('//td[@class="t_f"]/ignore_js_op')
        for each_block in block_list:
            # 链式调用xpath()
            # 获取单个图片的src
            each_src = each_block.xpath('./img/@zoomfile').extract_first()
            src_str = self.prefix_url + each_src
            # 获取单个图片的名称
            each_name = each_block.xpath('./div/div[@class="xs0"]/p[1]/strong/text()').extract_first()
            print(each_name, src_str)
```

运行结果如下图
![](resources/2023-07-18-01-19-41.png)
现在可以获得所有人物的各自的所有图片的名称和下载链接

## 保存统计信息到文件

统计：
1. 各个人物列表中的所有人物信息
2. 每个人物详细页的所有图片名和图片总数



## 下载图片到本地




