# 数据访问

JDBC、MyBatis、Spring Data JPA

![](resources/2023-02-19-23-29-59.png)

## JDBC

新建项目
![](resources/2023-02-19-23-33-56.png)
![](resources/2023-02-19-23-35-18.png)

启动数据库，确保其中有名为```atguigudb```的数据库

在配置文件```src/main/resources/application.yaml```中添加
```yaml
spring:
  datasource:
    username: root
    password: 1
    url: jdbc:mysql://localhost:3306/atguigudb
    # Mysql8.0以上版本的driver
    driver-class-name: com.mysql.cj.jdbc.Driver
```

```src/test/java/com/kzj/springboot06datajdbc/SpringBoot06DataJdbcApplicationTests.java```修改为
```java
package com.kzj.springboot06datajdbc;
import org.junit.jupiter.api.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@RunWith(SpringRunner.class)
@SpringBootTest
class SpringBoot06DataJdbcApplicationTests {
    @Autowired
    DataSource dataSource;

    @Test
    void contextLoads() throws SQLException {
        System.out.println(dataSource.getClass());
        Connection connection = dataSource.getConnection();
        System.out.println(connection);
        connection.close();
    }
}
 ```

测试方法的运行结果为
![](resources/2023-02-19-23-54-56.png)

### 自动配置原理

数据源的相关配置在```DataSourceConfiguration.class```里面
用```@Bean```向容器中添加各种```dataSource```组件，这个类主要是给容器添加各种**数据源**，也可以使用```spring.datasource.type```指定自定义的数据源类型，继续到```DataSourceBuilder.class```里面的```build()```方法，使用```BeanUtils```工具进行反射，创建相应type的数据源，并且绑定相关属性

数据源自动配置在```DataSourceAutoConfiguration.class```

到P61 7min
剩P62















## MyBatis

新建项目
![](resources/2023-02-21-23-36-08.png)
![](resources/2023-02-21-23-37-37.png)

![](resources/2023-02-21-23-40-47.png)







## Spring Data JPA

### 简介

![](resources/2023-02-20-00-01-42.png)
![](resources/2023-02-20-00-03-09.png)
![](resources/2023-02-20-00-05-03.png)
![](resources/2023-02-20-00-05-19.png)
![](resources/2023-02-20-00-07-01.png)

### 整合JPA

JPA:ORM（Object Relation Mapping、对象关系映射），是通过使用描述对象和数据库之间映射的元数据，将面向对象语言程序中的对象自动持久化到关系数据库中。简单来说就是将数据库表与java实体对象做一个映射

新建项目
![](resources/2023-02-20-00-09-20.png)
![](resources/2023-02-20-00-10-32.png)

同前面的项目，在配置文件```src/main/resources/application.yaml```中配置数据源
```yaml
spring:
  datasource:
    username: root
    password: 1
    url: jdbc:mysql://localhost:3306/atguigudb
    # Mysql8.0以上版本的driver
    driver-class-name: com.mysql.cj.jdbc.Driver
```

#### 编写一个实体类（bean）和数据表进行映射，并且配置好映射关系

新增```src/main/java/com/kzj/springboot06datajpa/entity/User.java```
```java
package com.kzj.springboot06datajpa.entity;
import javax.persistence.*;

// 使用JPA注解配置映射关系
@Entity     // 告诉JPA这是一个实体类（和数据表映射的类）
@Table(name = "tbl_user")   // 指定和哪个数据表对应，如果省略，默认表名是类名小写（user）
public class User {
    @Id     // 这是一个主键
    @GeneratedValue(strategy = GenerationType.IDENTITY)     // 自增策略
    private Integer id;
    @Column(name = "last_name", length = 50)     // 指定和数据表对应的一个列，如果省略，默认列名是属性名
    private String lastName;
    @Column
    private String email;
    // 省略的setter、getter方法
}
```
其中配置的数据表```tbl_user```不需要自己创建

#### 编写一个Dao接口（Repository）来操作实体类对应的数据表

新增```src/main/java/com/kzj/springboot06datajpa/repository/UserRepository.java```
继承```JpaRepository```
```java
package com.kzj.springboot06datajpa.repository;
import com.kzj.springboot06datajpa.entity.User;
import org.springframework.data.jpa.repository.JpaRepository;

// 继承JpaRepository来完成对数据库的操作
public interface UserRepository extends JpaRepository<User, Integer> {
}
```

#### 基本配置

现在还没有数据表```tbl_user```，也不需要自己创建
还需要对JPA做一些配置

```src/main/resources/application.yaml```修改为
```yaml
spring:
  datasource:
    username: root
    password: 1
    url: jdbc:mysql://localhost:3306/atguigudb
    # Mysql8.0以上版本的driver
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      # 定义数据表的生成策略，如果没有数据表，则会创建数据表；如果有数据表，则会更新数据表结构
      ddl-auto: update
    # 控制台显示sql
    show-sql: true
 ```

运行项目
![](resources/2023-02-20-00-57-46.png)
成功在数据库中建```tbl_user```表
![](resources/2023-02-20-00-58-38.png)

#### 增删改查

新增```src/main/java/com/kzj/springboot06datajpa/controller/UserController.java```
```java
package com.kzj.springboot06datajpa.controller;
import com.kzj.springboot06datajpa.entity.User;
import com.kzj.springboot06datajpa.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {
    @Autowired
    UserRepository userRepository;

    @GetMapping("/user/{id}")
    public User getUser(@PathVariable("id") Integer id){
        User user = userRepository.findById(id).get();
        return user;
    }

    @GetMapping("/user")
    public User insertUser(User user){
        // 返回的user中带有自增主键
        User save = userRepository.save(user);
        return save;
    }
}
```

启动项目
访问```localhost:8080/user/1```来查看数据
![](resources/2023-02-21-22-58-32.png)
现在数据库表中还没有数据，所以报错
![](resources/2023-02-21-22-58-56.png)

访问```localhost:8080/user?lastName=zhangsan&email=aa@qq.com```来新增数据
![](resources/2023-02-21-23-01-53.png)









---

到P68



