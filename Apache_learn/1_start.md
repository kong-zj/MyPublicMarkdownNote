
# 安装 apache 

```sudo apt-get update```
```sudo apt-get install apache2```

安装完成后我们在浏览器输入```localhost```
![](2023-01-03-16-48-03.png)
表示安装成功

注意：
apache和nginx中部署的网页文件都放在同一个目录```/var/www/html```

## 修改apache的端口（为了不和nginx冲突）

[网页链接](https://blog.csdn.net/haitunmin/article/details/74931617)

![](2023-01-03-22-16-29.png)

![](2023-01-03-22-18-08.png)

同时，还要在```/etc/apache2/sites-enabled/000-default.conf```中修改
![](2023-01-03-22-19-21.png)

![](2023-01-03-22-20-24.png)

重启服务
![](2023-01-03-22-21-47.png)

端口修改成功
![](2023-01-03-22-22-24.png)

# 安装php

```sudo apt install php libapache2-mod-php```

![](2023-01-03-16-54-40.png)

查看发现安装成功
![](2023-01-03-17-08-22.png)

## 启动php

使用命令```sudo a2enmod php7.4```启动php
![](2023-01-03-17-11-17.png)


## 测试php处理

![](2023-01-03-17-03-46.png)

但是用浏览器访问，只返回源代码，没有解析
![](2023-01-03-17-04-12.png)

### 修改配置文件

配置文件所在目录为```/etc/apache2```
配置文件名为```apache2.conf```
在文件中添加一行内容：```AddType application/x-httpd-php .php```

![](2023-01-03-17-26-10.png)
别忘了重启apache服务

用浏览器访问，成功
![](2023-01-03-17-28-41.png)

