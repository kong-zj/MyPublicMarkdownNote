# 流程控制

之前进行条件判断，就是为了流程控制

## if判断

![](resources/2023-01-15-21-32-45.png)

```;```的作用：分隔两个命令
![](resources/2023-01-15-21-34-43.png)

### 实例：判断登录的用户

shell脚本文件内容为
 ```shell
#!/bin/bash
if [ $1 = kzj ]
then
        echo "welcome, $1"
fi
 ```
如果传入参数为空，会报错
![](resources/2023-01-15-21-39-46.png)

所以，shell脚本文件内容改进为
 ```shell
#!/bin/bash
if [ "$1"x = "kzj"x ]
then
        echo "welcome, $1"
fi
 ```
这样，传入空值就不会报错了
![](resources/2023-01-15-21-43-48.png)

### 实例：多条件判断

shell脚本文件内容为
 ```shell
#!/bin/bash
if [ $1 -gt 18 ] && [ $1 -lt 35 ]
then
        echo OK
fi
 ```
![](resources/2023-01-15-21-50-54.png)

if后面能不能只用一对中括号，即把```&&```放到```[  ]```里面
```&&```不能直接放进去，换成```-a```可以放进去
```shell
#!/bin/bash
if [ $1 -gt 18 -a $1 -lt 35 ]
then
	echo OK
fi
 ```
![](resources/2023-01-15-21-55-24.png)

#### 布尔运算符

![](resources/2023-01-15-21-56-45.png)

### if多分支

之前都是if单分支，现在开始if多分支

shell脚本文件内容为
 ```shell
#!/bin/bash
if [ $1 -lt 18 ]
then
	echo "kid or teenager"  
elif [ $1 -lt 60 ]
then
	echo "adult"
else
	echo "older"
fi
 ```
![](resources/2023-01-15-22-08-52.png)

## case语句

![](resources/2023-01-15-22-14-21.png)

shell脚本文件内容为
 ```shell
#!/bin/bash
case $1 in
1)
	echo "one"
;;
2)
	echo "two"
;;
*)
	echo "number else"
;;
esac
```
![](resources/2023-01-15-22-18-24.png)

## for循环








---

到P77
