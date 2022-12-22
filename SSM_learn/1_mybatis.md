# MyBatis

## 简介

持久层框架
DAO：数据库访问对象
ORM：对象关系映射（对象指**Java中的对象**，关系指**关系型数据库**，创建Java对象和数据库记录的映射）
![](2022-12-21-23-26-16.png)

1. 纯手动：JDBC
2. 半自动：MyBatis
3. 全自动：Hibernate

![](2022-12-21-23-35-25.png)

## 安装

首先用Maven构建一个工程，并引入依赖
```xml
 <!--依赖列表-->
  <dependencies>
 
    <!--MyBatis依赖-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.7</version>
    </dependency>
 
    <!--mysql驱动-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.16</version>
    </dependency>
 
    <!--单元测试-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
```

![](2022-12-21-23-49-26.png)

在Vscode中可以看到
![](2022-12-22-10-27-57.png)

## 简单使用（在MyBatis中面向接口，实现sql语句的执行）

### 在数据库中建表

新建名为SSM的数据库，在其中新建表t_user
![](2022-12-22-10-26-07.png)

### 创建对应的实体类

User.java文件
![](2022-12-22-11-08-29.png)
这里的属性名要对应数据库中的列名

用右键```源代码操作```
![](2022-12-22-10-39-32.png)

增加
1. get，set方法
2. 构造方法（无参构造方法必须要有）
3. toString方法
![](2022-12-22-10-40-49.png)


### 创建mybatis的核心配置文件（如何连接数据库）

![](2022-12-22-10-46-26.png)

mybatis-config.xml的文件内容形式如下
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <!--配置数据源：创建Connection对象-->
            <dataSource type="POOLED">
                <!--driver：驱动内容-->
                <property name="driver" value="${driver}"/>
                <!--连接数据库的url-->
                <property name="url" value="${url}"/>
                <!--用户名-->
                <property name="username" value="${username}"/>
                <!--密码-->
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
 
    <!--指定mybatis的mapper映射文件的位置
        目的是找到其他mapper文件的sql语句
    -->
    <mappers>
        <!--使用mapper的resource属性指定mapper文件的路径(使用 / 分割路径)
            这个路径是相对于src/main/java/的
            一个resource指定一个mapper文件
        -->
        <mapper resource="{mybatis的mapper映射文件}"/>
    </mappers>
</configuration>
```
注意这里的mapper映射文件此时还没有编写，之后创建mybatis的mapper映射文件（如何操作数据库）再回来填写

#### 我的mybatis-config.xml文件内容（UserMapper.xml文件创建后）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <!--配置数据源：创建Connection对象-->
            <dataSource type="POOLED">
                <!--driver：驱动内容-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <!--连接数据库的url-->
                <property name="url" value="jdbc:mysql://172.23.215.121:3306/SSM?serverTimezone=UTC"/>
                <!--用户名-->
                <property name="username" value="root"/>
                <!-- 密码 -->
                <property name="password" value="1"/>
            </dataSource>
        </environment>
    </environments>
 
    <!--指定mybatis的mapper映射文件的位置
        目的是找到其他mapper文件的sql语句
    -->
    <mappers>
        <!--使用mapper的resource属性指定mapper文件的路径(使用 / 分割路径)
            这个路径是相对于src/main/resources/的
            一个resource指定一个mapper文件
        -->
        <mapper resource="mappers/UserMapper.xml"/>
    </mappers>
</configuration>
```

#### 注意这里路径的配置（1）

```<mapper resource="{mybatis的mapper映射文件}"/>```
这个路径是从src/main/resources/路径开启的，即写的路径是**相对于resources文件夹的**

### 创建mapper接口

注意，mapper接口是```.java```文件

![](2022-12-22-11-02-50.png)

我的UserMapper.java文件内容如下
![](2022-12-22-11-07-14.png)

### 创建mybatis的mapper映射文件（如何操作数据库）

注意，mapper映射文件是```.xml```文件

![](2022-12-22-11-16-24.png)

注意：
1. mapper接口文件（UserMapper.java）和mapper映射文件（UserMapper.xml）要保证两个一致
2. 这里为了方便管理（容易看出他们的对应关系），接口文件和映射文件的文件名相同，只有后缀不同
![](2022-12-22-14-35-43.png)

UserMapper.xml的文件内容形式如下
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="{第1个一致：这里要与Mapper接口的全类名保持一致}">
    <insert id="{第2个一致：这里要与Mapper接口中的方法名一致}">
        insert into t_user values(null, 'admin', '123456', 23, '男', '12345@qq.com')
    </insert>
</mapper>
 ```
在映射文件中，让mapper接口中的一个方法对应一个sql语句

#### 我的UserMapper.xml文件内容（UserMapper.java文件创建后）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.mybatis.mapper.UserMapper">
    <insert id="insertUser">
        insert into t_user values(null, 'admin', '123456', 23, '男', '12345@qq.com')
    </insert>
</mapper>
```
当调用UserMapper接口中的insertUser方法时：
1. 通过mapper接口的全类名，找到对应的mapper映射文件
2. 通过调用的方法名，找到映射文件中的sql语句

### 创建测试类（用得多）

![](2022-12-22-17-18-58.png)
- sql语句和mapper接口中的方法对应，现在要创建mapper接口的对象，调用其中的方法，但是接口不能直接创建实例化对象
- 这时用**sqlSession的getMapper方法**，传入某个类型的class对象，会返回这个类型的实例化对象（这个方法的底层使用的是**代理模式**）
- 用代理模式创建了当前接口的**代理实现类**，这个实现类肯定实现了UserMapper接口，那他就一定帮助我们重写接口的方法，怎们**重写方法**，就是之前提到的：
> 当调用UserMapper接口中的insertUser方法时：
>1. 通过mapper接口的全类名，找到对应的mapper映射文件
>2. 通过调用的方法名，找到映射文件中的sql语句

经测试，得到输出
![](2022-12-22-17-26-08.png)

但是数据库表中没有插入的信息
![](2022-12-22-17-22-17.png)

这是因为如果以这种方式创建sqlSession，通过sqlSession执行一个sql语句的话，我们**必须自己设置事务的提交和回滚**，他不会自动提交，默认是回滚的，所以数据库中看不到效果

如下图，加一行```sqlSession.commit();```即可
![](2022-12-22-17-32-43.png)

成功写入数据库
![](2022-12-22-17-34-09.png)

#### 注意这里路径的配置（2）

```InputStream is = Resources.getResourceAsStream("mybatis-config.xml");```
这个路径是从src/main/resources/路径开启的，即写的路径是**相对于resources文件夹的**

### 测试类中的另一种写法（通过sql的唯一标识）（用得不多）

通过提供sql语句的唯一标识，找到sql并执行
唯一标识```namespace.sqlId```是```com.example.mybatis.mapper.UserMapper.insertUser```
> ```namespace```在
> ![](2022-12-22-17-55-15.png)
> ```sqlId```在
> ![](2022-12-22-17-57-13.png)

测试类如下图
![](2022-12-22-18-00-08.png)

成功写入数据库
![](2022-12-22-17-48-41.png)

#### 让sqlSession自动提交

在openSession中传入true，获取到的sqlSession对象就可以自动提交事务
![](2022-12-22-22-19-50.png)

### 加入log4j日志功能

#### 在pom.xml文件中加入依赖

![](2022-12-22-22-25-32.png)

#### 加入log4j的配置文件log4j.xml

配置文件log4j.xml存放在src/main/resources目录下
log4j.xml文件内容如下
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
 
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
	<appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
		<param name="Encoding" value="UTF-8" />
		<layout class="org.apache.log4j.PatternLayout">
			<param name="ConversionPattern" value="%d{HH:mm:ss,SS} %-5p (%C{1}:%M) - %m%n"/>
		</layout>
	</appender>

	<logger name="java.sql">
		<level value="debug"/>
	</logger>
	
	<logger name="com.apache.ibatis">
		<level value="info"/>
	</logger>
	
	<root>
		<level value="debug"/>
		<appender-ref ref="STDOUT" />
	</root>
</log4j:configuration>
 ```

 再次执行测试类，会输出日志信息
 ![](2022-12-22-23-06-46.png)

 #### 日志级别
 
 ![](2022-12-22-23-41-59.png)

### 源码验证：代理实现类对象userMapper的insertUser方法的实现方式

- sql语句和mapper接口中的方法对应，现在要创建mapper接口的对象，调用其中的方法，但是接口不能直接创建实例化对象
- 这时用**sqlSession的getMapper方法**，传入某个类型的class对象，会返回这个类型的实例化对象（这个方法的底层使用的是**代理模式**）
- 用代理模式创建了当前接口的**代理实现类**，这个实现类肯定实现了UserMapper接口，那他就一定帮助我们重写接口的方法，怎们**重写方法**，就是之前提到的：
> 当调用UserMapper接口中的insertUser方法时：
>1. 通过mapper接口的全类名，找到对应的mapper映射文件
>2. 通过调用的方法名，找到映射文件中的sql语句

打断点
![](2022-12-22-23-18-14.png)

启动debug
![](2022-12-22-23-23-32.png)

点击**单步调试**
![](2022-12-22-23-25-24.png)

在invoke方法中
![](2022-12-22-23-28-41.png)

在execute方法中
![](2022-12-22-23-34-46.png)

可见，把要执行的sql语句封装到command中
![](2022-12-22-23-35-55.png)

sqlSession.insert方法，通过提供sql语句的唯一标识，找到sql并执行，和我们自己在测试类中写的比较像
![](2022-12-22-23-37-47.png)

### 加入修改、删除、查询功能

P12


