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

### 在Vsocde中配置Maven

![](2022-12-21-13-45-44.png)

### 配置阿里云镜像

在<mirrors></mirrors>标签中添加 mirror 子节点
```
<mirror>
  <id>aliyunmaven</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun maven</name>
  <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```
![](2022-12-21-16-07-23.png)
![](2022-12-21-16-07-06.png)
这样就配置成功

# 构建Maven工程

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

## 快速管理Maven工程（Vscode中的插件）

![](2022-12-21-13-49-28.png)
   
# Maven标准目录结构

![](2022-12-21-13-39-24.png)

![](2022-12-21-13-42-33.png)

# 运行Maven构建的web工程（通过Tomcat）

## 添加Tomcat插件（注意*插件*和*依赖*的坐标，添加入pom.xml的位置不同）

到[Maven中央仓库](https://mvnrepository.com/)找到这个插件
![](2022-12-21-14-05-38.png)
![](2022-12-21-14-06-46.png)

复制其中的内容
```
<!-- https://mvnrepository.com/artifact/org.apache.tomcat.maven/tomcat7-maven-plugin -->
<dependency>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
</dependency>
```
到pom.xml文件中

具体位置如下图
![](2022-12-21-14-09-48.png)

只要保留groupId，artifactId，version 三项
![](2022-12-21-14-53-42.png)

问题：加上这个内容之后，预计会在Vscode中看到这个插件，结果并没有（刷新也没用）
![](2022-12-21-14-50-04.png)

但是在本地仓库中可以找到刚才下载的插件
![](2022-12-21-14-42-02.png)

原因：pluginManagement标签锁定插件版本（自动创建时带的）
[解决方法：](https://blog.csdn.net/Laputa219/article/details/102638415)

更换tomcat坐标放的位置，不要用```pluginManagement```标签括住我们刚刚添加的Tomcat的坐标
![](2022-12-21-14-55-40.png)

这时会在Vscode中看到这个插件
![](2022-12-21-14-58-58.png)

## 启动Tomcat插件

问题：此时，在Vscode中直接点击run，会报错
![](2022-12-21-15-06-35.png)

应该是下图中划线处```undefined```的问题，原因是在Vscode中没有配置好
![](2022-12-21-15-07-59.png)

把```mvn undefined:run -f "/home/kzj/project/maven_study/webdemo/pom.xml"```
改成```mvn tomcat7:run -f "/home/kzj/project/maven_study/webdemo/pom.xml"```
放在命令行终端中可以正常运行
![](2022-12-21-15-13-47.png)
![](2022-12-21-15-15-27.png)

解决方法：在IDEA中是正常的
![](2022-12-21-15-24-24.png)
暂时不要在Vscode直接点击run，手动命令行代替

## 配置Tomcat插件

在```pom.xml```文件的Tomcat中添加如下信息
![](2022-12-21-15-33-46.png)
其中port是配置端口号，path是配置项目的虚拟路径

使用```mvn tomcat7:run -f "/home/kzj/project/maven_study/webdemo/pom.xml"```命令，启动Tomcat
![](2022-12-21-15-30-31.png)

![](2022-12-21-15-37-47.png)    

# pom.xml文件的结构

![](2022-12-21-15-44-41.png)

## Maven中依赖（dependency）和插件（plugin）的区别

> 依赖：运行时开发时都需要用到的jar包，比如项目中需要一个Json的jar包，就要添加一个依赖，这个依赖在项目运行时也需要，因此在项目打包时需要把这些依赖也打包进项目里；
> 插件：在项目开的发时需要，但是在项目运行时不需要，因此在项目开发完成后不需要把插件打包进项目中，比如有个可以自动生成getter和setter的插件，因为这玩意在编译时生成getter和setter，编译结束后就没用了，所以项目打包时并不需要把插件放进去
