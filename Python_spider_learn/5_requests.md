# selenium 库

和 urllib 库作用类似

## 基本使用

![](resources/2023-06-23-00-34-44.png)

### 安装

```shell
pip install requests
```

### 一个类型和六个属性


#### Response 类型

```py
import requests
url = 'http://www.baidu.com'
response = requests.get(url=url)
print(type(response))
# 打印输出为<class 'requests.models.Response'>
```

#### 六个属性

```py
import requests
url = 'http://www.baidu.com'
response = requests.get(url=url)
# 设置响应的编码格式
response.encoding = 'utf-8'
# 六个属性
print(response.text)
print(response.encoding)
print(response.url)
print(response.content)
print(response.status_code)
print(response.headers)
```

### urllib 库和 requests 库学习路线对比

![](resources/2023-06-23-00-54-21.png)

## get请求

用百度搜索北京，url显示为
```url
https://www.baidu.com/s?wd=北京
```
实际编码为
```url
https://www.baidu.com/s?wd=%E5%8C%97%E4%BA%AC
```

发起get请求的代码为
```py
import requests
# url = 'https://www.baidu.com/s?'
url = 'https://www.baidu.com/s'
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36 Edg/114.0.1823.51'
}
data = {
    'wd': '北京'
}

# requests.get()的传入参数
# url 请求资源路径
# params 参数
# kwargs 字典
response = requests.get(url=url, params=data, headers=headers)
content = response.text
print(content)

# get请求总结：
# 1.参数使用params传递
# 2.参数无需像urllib中使用urlencode编码
# 3.不需要请求对象的定制
# 4.请求资源路径中的？可加可不加
``` 

## post请求

为了方便和urllib库的使用做对比，还是用之前的百度翻译案例

```py
import requests
url = 'https://fanyi.baidu.com/sug'
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36 Edg/114.0.1823.43'
}
data = {
    'kw': 'spider',
}

# requests.post()的传入参数
# url 请求地址
# data 请求参数
# kwargs 字典
response = requests.post(url=url, data=data, headers=headers)
content = response.text
print(content)
# 字符串 -> json对象
import json
obj = json.loads(content)
print(type(obj))
print(obj)

# post请求总结：
# 1.不需要编码解码
# 2.参数使用data传递
# 3.不需要请求对象的定制
```

## 代理










---
到p87