# 正则表达式

[正则表达式在线测试网站](https://c.runoob.com/front-end/854/)

![](resources/2023-01-19-22-24-08.png)

awk、sed、grep更适合的方向：
- ```grep``` 更适合单纯的查找或匹配文本
- ```sed``` 更适合编辑匹配到的文本
- ```awk``` 更适合格式化文本，对文本进行较复杂格式处理

[Linux grep 命令 菜鸟教程](https://www.runoob.com/linux/linux-comm-grep.html)

[Linux sed 命令 菜鸟教程](https://www.runoob.com/linux/linux-comm-sed.html)

[Linux awk 命令 菜鸟教程](https://www.runoob.com/linux/linux-comm-awk.html)

## 常用特殊字符

### ```^```

匹配一行的开头

### ```$```

匹配一行的结尾
![](resources/2023-01-19-22-43-03.png)

放一起用```^$```就是匹配空行
![](resources/2023-01-19-22-45-55.png)

### ```.```

匹配一个任意的字符

### ```*```

表示上一个字符出现任意次（0次、1次、多次），不会单独使用
![](resources/2023-01-19-22-51-06.png)

```.*```可以匹配任意
![](resources/2023-01-19-22-54-24.png)

### ```[]```

匹配某个范围内的一个字符
![](resources/2023-01-19-22-56-41.png)

![](resources/2023-01-19-22-59-10.png)

### ```\```

表示转义，不会单独使用
![](resources/2023-01-19-23-02-26.png)
注意：必须用**单引号**才能表示转义，不能用双引号

## 扩展的正则表达式

### ```{}```

指定上一个字符出现的次数，不会单独使用

### ```+```

类似```*```，但表示一次或多次

### ```?```

类似```*```，但表示0次或一次

## 实践：匹配手机号

![](resources/2023-01-19-23-11-48.png)
默认不支持扩展的特殊字符```{}```，要用```-E```参数，表示支持扩展的正则表达式

# 文本处理工具

## cut

[Linux cut 命令 菜鸟教程](https://www.runoob.com/linux/linux-comm-cut.html)

![](resources/2023-01-19-23-24-18.png)

用```ifconfig eth0 | grep "inet " | cut -d " " -f 10```命令得到当前的IP地址
```grep```相当于提取选定的**行**，```cut```相当于提取选定的**列**
![](resources/2023-01-19-23-28-44.png)
注意这里```-f 10```的原因是，那一行的前面有很多空格，要数清楚

如果要切出所有的IP地址，就用命令```ifconfig | grep "inet " | cut -d " " -f 10```

## awk

[Linux awk 命令 菜鸟教程](https://www.runoob.com/linux/linux-comm-awk.html)

![](resources/2023-01-19-23-36-23.png)
注意```which```与```whereis```的区别：
1. ```which```查找的可执行文件，必须是要**在PATH下的可执行文件**
2. ```whereis```可以用来查找二进制（命令）、源文件、man文件。与which不同的是这条命令可以是**通过文件索引数据库而非PATH来查找的**，所以查找的面比which要广

![](resources/2023-01-19-23-39-20.png)








到P86





