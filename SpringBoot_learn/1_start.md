# SpringBoot

## 学习路径

![](resources/2023-02-11-15-37-54.png)

## 简介

![](resources/2023-02-11-22-33-18.png)

## 微服务

![](resources/2023-02-11-22-38-58.png)

![](resources/2023-02-11-22-40-45.png)

[微服务介绍](https://martinfowler.com/microservices/)

一个应用应该是一组小型服务，可以通过HTTP的方式进行互通

## 环境准备

![](resources/2023-02-11-22-53-50.png)

### maven设置

给maven的```settings.xml```配置文件的```profiles```标签添加
```xml
    <profile> <!--配置springboot创建maven时 设置默认的编译环境  -->
      <id>jdk-1.8</id>
        <activation>
            <activeByDefault>true</activeByDefault>
            <jdk>1.8</jdk>
        </activation>
          <properties>
            <maven.compiler.source>1.8</maven.compiler.source>
            <maven.compiler.target>1.8</maven.compiler.target>
            <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion> 
         </properties>
    </profile>
```

### IDEA设置

配置成自己的maven
![](resources/2023-02-11-23-47-52.png)


