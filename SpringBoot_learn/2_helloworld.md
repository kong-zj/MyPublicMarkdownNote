# HelloWorld

## 创建一个maven工程（jar）

![](resources/2023-02-11-23-17-36.png)

## 导入springboot依赖

在```pom.xml```文件中添加
```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

## 编写一个主程序，用于启动springboot应用

创建文件```src/main/java/com/kzj/HelloWorldMainApplication.java```内容如下
```java
package com.kzj;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class HelloWorldMainApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloWorldMainApplication.class, args);
    }
}
```

## 编写相关的Controller、Service

创建文件```src/main/java/com/kzj/ccontroller/HelloController.java```内容如下
```java
package com.kzj.ccontroller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {
    @ResponseBody
    @RequestMapping("/hello")
    public String hello(){
        return "HelloWorld";
    }
}
```

## 运行主程序的main方法

![](resources/2023-02-12-00-04-11.png)

![](resources/2023-02-12-00-05-15.png)

![](resources/2023-02-12-00-05-39.png)

![](resources/2023-02-12-00-06-11.png)

## 简化部署

不需要打war包，创建可执行的jar包，无需在目标服务器安装tomcat

### 导入springboot的maven插件

在```pom.xml```文件中添加
```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

### 使用插件打包

![](resources/2023-02-12-00-12-47.png)

![](resources/2023-02-12-00-17-30.png)

### 运行jar包

![](resources/2023-02-12-00-20-45.png)

经测试，成功
![](resources/2023-02-12-00-06-11.png)

## pom.xml文件探究

### 父项目

![](resources/2023-02-12-00-31-39.png)

### springboot场景启动器

![](resources/2023-02-12-00-35-50.png)





---
到P7




