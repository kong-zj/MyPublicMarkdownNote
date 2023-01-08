# Nginx

## 基本概念

Nginx (engine x) 是一个高性能的HTTP和反向代理web服务器。
Nginx是一款轻量级的Web服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，在BSD-like协议下发行。其特点是占有内存少，并发能力强。

### 反向代理

正向代理：
正向代理总结就一句话：代理端**代理的是客户端**。
![](resources/2023-01-02-21-20-25.png)

反向代理：
反向代理总结就一句话：代理端**代理的是服务端**。
![](resources/2023-01-02-21-22-27.png)

### 负载均衡

![](resources/2023-01-02-21-28-39.png)

![](resources/2023-01-02-21-30-37.png)

### 动静分离

![](resources/2023-01-02-21-34-16.png)

![](resources/2023-01-02-21-34-32.png)

## 安装Nginx

安装依赖：
依赖包openssl安装```sudo apt-get install openssl libssl-dev```
依赖包pcre安装```sudo apt-get install libpcre3 libpcre3-dev```
依赖包zlib安装```sudo apt-get install zlib1g-dev```

安装Nginx：
```sudo apt update```
```sudo apt install nginx```

启动Nginx
![](resources/2023-01-02-21-54-10.png)

使用```whereis```命令，可以查找已安装软件的位置
![](resources/2023-01-02-21-59-42.png)

配置文件所在位置为
![](resources/2023-01-02-22-01-17.png)
配置文件详解见```2_nginxconf.md```

通过WSL子系统的IP地址和80端口号
![](resources/2023-01-02-22-03-09.png)

成功访问
![](resources/2023-01-02-22-03-53.png)

### 浏览器中访问nginx的端口，出现的却是apache的欢迎页

已经将apache的端口修改为：
1. 80->90
2. 443->444

访问nginx的80端口，却显示apache的欢迎页（在安装完apache后出现此现象）
![](resources/2023-01-03-22-43-10.png)

[网页链接](https://blog.csdn.net/jishuai6p/article/details/120335376)

注意：
apache与nginx是共用同一个站点目录的，apache和nginx中部署的网页文件都放在同一个目录```/var/www/html```

在nginx的配置文件```/etc/nginx/sites-available/default```中：
1. 第22-23行可修改nginx的端口号
2. 第41行用来设置nginx自己的站点目录
3. 第44行是根据所排列的顺序调用站点目录里的网页文件作为自己的欢迎页(解决一开始的问题只需把index.nginx-debian.html放在index.html前就行了)
![](resources/2023-01-03-22-38-26.png)

第44行改为```index index.nginx-debian.html index.html index.htm;```，并重启nginx服务，问题解决
![](resources/2023-01-03-22-49-50.png)

## 常用命令

### 查看nginx版本号

![](resources/2023-01-02-22-08-54.png)

### 关闭nginx

![](resources/2023-01-02-22-10-11.png)

### 启动nginx

![](resources/2023-01-02-22-11-02.png)

### 重加载nginx

让修改的配置文件生效
![](resources/2023-01-02-22-25-28.png)

用Ubuntu的service命令也可以实现类似的效果
![](resources/2023-01-02-22-28-09.png)


