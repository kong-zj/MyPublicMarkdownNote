# MyBatis核心配置文件

MyBatis核心配置文件中的标签必须按照指定的顺序：
> properties?,settings?,typeAliases?,typeHandlers?,
> objectFactory?,objectWrapperFactory?,reflectorFactory?,
> plugins?,environments?,databaseIdProvider?,mappers?

## ```<environments>```标签

在**mybatis-config.xml文件**中
使用```<environments>```标签，**配置连接数据库的环境**
这里有两个环境，一个开发，一个测试
```xml
    <environments default="development">
        <environment id="development">
            <!-- 
                transactionManager：设置事务管理器
                属性type="JDBC / MANAGED"：设置事务管理的方式，
                    其中JDBC：使用JDBC中原生的事务管理方式，
                    MANAGED：被管理，例如Spring
             -->
            <transactionManager type="JDBC"/>
            <!--
                dataSource：配置数据源
                属性type="POOLED / UNPOOLED / JNDI"：设置数据源的类型，
                    其中POOLED：使用数据库连接池
                    UNPOOLED：不使用数据库连接池
                    JNDI：使用上下文中的数据源
            -->
            <dataSource type="POOLED">
                <!--driver：驱动内容-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <!--连接数据库的url-->
                <property name="url" value="jdbc:mysql://localhost:3306/SSM?serverTimezone=UTC"/>
                <!--用户名-->
                <property name="username" value="root"/>
                <!-- 密码 -->
                <property name="password" value="1"/>
            </dataSource>
        </environment>

        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/SSM?serverTimezone=UTC"/>
                <property name="username" value="root"/>
                <property name="password" value="1"/>
            </dataSource>
        </environment>
    </environments>
```

## ```<properties>```标签

首先在mybatis-config.xml文件的同目录下创建jdbc.properties文件，在里面写键值对
**jdbc.properties文件**的内容为
```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/SSM?serverTimezone=UTC
jdbc.username=root
jdbc.password=1
```

现在还不能在核心配置文件mybatis-config.xml中访问
需要把properties引入配置文件
在**mybatis-config.xml文件**中
使用```<properties>```标签，**引入jdbc.properties文件**
然后就可以用**使用${key}的方式访问value**（这里的key和value都是jdbc.properties文件中的）
```xml
    <!-- 引入properties文件，此后就可以在当前文件中使用${key}的方式访问value -->
    <properties resource="jdbc.properties" />

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>

        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
```

## ```<typeAliases>```标签

在**mybatis-config.xml文件**中
使用```<typeAliases>```标签，**设置类型别名**
```xml
    <typeAliases>
        <!--
            typeAlias：设置类型别名
            属性type：设置需要起别名的类型
            属性alias：别名
        -->
        <typeAlias type="com.example.mybatis.pojo.User" alias="abc"></typeAlias>
    </typeAliases>
```

然后就可以在MyBatis的范围中，使用别名表示具体的类型
在**UserMapper.xml文件**（mapper映射文件）中
可以**用别名abc代替原名com.example.mybatis.pojo.User**
```xml
    <select id="getUserById" resultType="abc">
        select * from t_user where id = 35
    </select>

    <select id="getAllUser" resultType="abc">
        select * from t_user
    </select>
```
### 如果不设置属性alias

在**mybatis-config.xml文件**改成
```xml
    <typeAliases>
        <!--
            如果不设置alias属性，这个类型会拥有默认的别名
            默认的别名就是类名，且不区分大小写
        -->
        <typeAlias type="com.example.mybatis.pojo.User"></typeAlias>
    </typeAliases>
```

则**用默认的别名User（不区分大小写）代替原名com.example.mybatis.pojo.User**

在**UserMapper.xml文件**（mapper映射文件）改成
```xml
    <select id="getUserById" resultType="User">
        select * from t_user where id = 35
    </select>

    <select id="getAllUser" resultType="User">
        select * from t_user
    </select>
```
### 使用```<package>```标签

如果有很多实体类，使用上面的```<typeAlias>```标签，一次只能给一个**实体类**设置别名，那```<typeAlias>```需要写很多才行

改进：
使用```<package>```标签，以**包**（所有的实体类统一放在一个包下）的方式设置别名
在**mybatis-config.xml文件**改成
```xml
    <typeAliases>
        <!-- 
            此时这个包下面的所有实体类将全部拥有默认的别名（即类名且不区分大小写）
        -->
        <package name="com.example.mybatis.pojo" />
    </typeAliases>
```



到sgg P17


