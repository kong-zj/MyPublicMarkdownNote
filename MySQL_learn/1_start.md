# MySQL

### 基础篇大纲

![](2022-07-31-15-01-07.png)

### 外部资源

[官方文档](https://dev.mysql.com/doc/refman/8.0/en/)
[数据结构可视化](https://www.cs.usfca.edu/~galles/visualization/about.html)

### 书

![](2022-07-31-15-10-25.png)
![](2022-07-31-15-11-02.png)

### 数据库分类

#### RDBMS与非RDBMS

##### RDBMS

![](2022-12-08-12-55-44.png)

##### 非RDBMS

![](2022-12-08-12-58-49.png)
![](2022-07-31-15-14-10.png)

### 表（RDBMS中）

![](2022-12-08-13-02-37.png)

#### 表的四种关联关系

![](2022-07-31-15-15-22.png)
![](2022-12-08-13-05-48.png)
![](2022-12-08-13-07-37.png)
![](2022-12-08-13-08-42.png)

### 总结

![](2022-07-31-15-18-19.png)

## MySQL启动

这里passwd是```1```
![](2022-12-08-14-59-24.png)
访问本机的MySQL时，有些参数可以省略

还可以加更多的参数
![](2022-12-08-15-03-31.png)

### 把wsl中的mysql端口映射到外部的windows中（使用Navicat连接）

首先需要改变MySQL的配置,执行```sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf```

找到 ```bind-address = 127.0.0.1``` 并注释掉 → ```# bind-address = 127.0.0.1```
![](2022-12-20-10-00-27.png)
```127.0.0.1```被称为[本地环回地址](https://blog.csdn.net/weixin_42204641/article/details/83548922)(loopback)

执行```sudo service mysql restart```重启 MySQL 服务

当你的帐号不允许从远程登陆，只能在localhost连接时。这个时候只要在mysql服务器上，更改 mysql 数据库里的 user 表里的 host 项，从localhost"改成%即可实现用户远程登录

在安装mysql的机器上运行：

1. mysql -u root -p  
2. use mysql;
3. select host,user from user where user='root';
4. update user set host = '%' where user='root' and host='localhost';  
5. select host, user from user where user='root';
![](2022-12-20-10-16-30.png)

在WSL中使用```ifconfig```命令
![](2022-12-20-10-25-58.png)

在windows的Navicat中添加连接，这里的ip地址使用ifconfig后查找的ip地址
![](2022-12-20-10-27-50.png)
即成功连接

### 启下

![](2022-07-31-15-20-11.png)

![](2022-12-08-15-24-46.png)