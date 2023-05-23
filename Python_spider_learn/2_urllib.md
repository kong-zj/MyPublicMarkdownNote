# urllib 库

## 基本使用

```py
import urllib
url = 'http://www.baidu.com'
# 模拟浏览器发送请求
response = urllib.request.urlopen(url)
# 获取相应的页面的源码
content = response.read().decode('utf-8')
# read方法返回的是二进制数据，还要将二进制数据转换为字符串（解码）（用decode）
print(content)
```

### 一个类型和六个方法

![](resources/2023-05-23-23-15-54.png)

#### HTTPResponse

```py
import urllib
url = 'http://www.baidu.com'
# 模拟浏览器发送请求
response = urllib.request.urlopen(url)
print(type(response))
# 打印输出为<class 'http.client.HTTPResponse'>
```

#### 六个方法

```py
import urllib
url = 'http://www.baidu.com'
response = urllib.request.urlopen(url)
content = response.read(10)
content = response.readline()
content = response.readlines()
code = response.getcode()
url2 = response.geturl()
headers = response.getheaders()
```

## 下载

### 下载网页

```py
import urllib.request
url_page = 'http://www.baidu.com'
# 保存到baidu.html文件中
urllib.request.urlretrieve(url_page, 'baidu.html')
```

### 下载图片

![](resources/2023-05-24-00-03-23.png)

```py
import urllib.request
url_img = 'https://avatars.githubusercontent.com/u/47291826?v=4'
urllib.request.urlretrieve(url_img, 'sky.jpg')
```

### 下载视频

![](resources/2023-05-24-00-13-46.png)

```py
import urllib.request
url_video = 'https://7m5.oss-cn-hangzhou.aliyuncs.com/upload/image/68499e58090d4aafa0fb6d0545d4f2c1/2023-03-29/2023-03-29279859.mp4'
urllib.request.urlretrieve(url_video, 'kgu.mp4')
```

## 请求对象的定制

![](resources/2023-05-24-00-37-08.png)

### url的组成

[url的组成链接](https://developer.mozilla.org/zh-CN/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL)

![](resources/2023-05-24-00-18-14.png)

### User-Agent

[UA大全链接](http://useragent.kuzhazha.com/)

![](resources/2023-05-24-00-23-55.png)

```py
import urllib.request
url = 'http://www.baidu.com'
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36 Edg/113.0.1774.50'
}
# 请求对象的定制
# 需要关键字传参
request = urllib.request.Request(url = url, headers = headers)
# 这样会报错，因为传递的参数的顺序问题
# request = urllib.request.Request(url, headers)
response = urllib.request.urlopen(request)
content = response.read().decode('utf8')
print(content)
```

第三个参数才是headers，如果在传参时不加指定，会把实参headers传递给形参data
![](resources/2023-05-24-00-45-17.png)

## 编码解码







---
到p57



