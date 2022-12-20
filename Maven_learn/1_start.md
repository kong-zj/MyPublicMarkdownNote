# 目前掌握的技术

![](2022-12-18-21-09-07.png)

## 存在的问题

![](2022-12-18-21-24-14.png)
![](2022-12-18-21-25-29.png)
![](2022-12-18-21-25-56.png)

# Maven是什么

项目构建工具

![](2022-12-18-21-30-50.png)
![](2022-12-18-21-31-57.png)

![](2022-12-20-22-04-25.png)
![](2022-12-20-22-06-45.png)

## 仓库

![](2022-12-20-22-17-22.png)
![](2022-12-20-22-18-08.png)

## 坐标

![](2022-12-20-22-29-02.png)

[中央仓库网址](https://mvnrepository.com/)

举例：
找到JUnit
![](2022-12-20-22-25-28.png)
点击4.12版本
![](2022-12-20-22-26-24.png)
其中
```
<!-- https://mvnrepository.com/artifact/junit/junit -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```
就是他的坐标

# 安装Maven

![](2022-12-20-22-10-45.png)

## 配置Maven

找到Maven的配置文件
![](2022-12-20-22-33-43.png)
可以看到:
1. Maven的```.m2```本地仓库的位置为```/home/kzj/.m2/repository```
2. Maven的配置文件的位置为```/usr/share/maven/conf/settings.xml```

# 构建Java项目

在Vscode中
使用快捷键：Ctrl+Shift+P，输入create Java project
选择用Maven创建
![](2022-12-18-21-38-16.png)

目录结构如下
![](2022-12-20-22-54-34.png)
其中```pom.xml```就是Maven的配置文件，用以描述项目的各种信息

![](2022-12-20-23-04-43.png)
![](2022-12-20-23-02-34.png)
可以看出一个坐标由以下三部分组成：
1. groupId
2. artifactId
3. version
   







