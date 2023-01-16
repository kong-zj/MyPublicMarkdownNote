# 读取控制台输入

## read

![](resources/2023-01-16-21-49-25.png)

shell脚本文件内容为
```shell
#!/bin/bash
read -t 10 -p "input your name: " name
echo "welcome, $name"
```
![](resources/2023-01-16-21-53-15.png)

# 函数（类似脚本文件）

## 系统函数

### ```date```

![](resources/2023-01-16-22-00-03.png)

系统函数的使用：```$()```（命令替换）（得到括号内命令的执行结果）
![](resources/2023-01-16-22-02-12.png)

### ```basename```

```$0```表示当前的脚本名称，shell脚本文件内容为
 ```shell
#!/bin/bash
echo script name: $0
```
![](resources/2023-01-16-22-11-04.png)

怎样可以不加前面的完整路径
![](resources/2023-01-16-22-12-45.png)
本质就是字符串的截取

![](resources/2023-01-16-22-15-36.png)
![](resources/2023-01-16-22-16-08.png)

### ```dirname```

![](resources/2023-01-16-22-17-38.png)
![](resources/2023-01-16-22-18-14.png)

### 实例：输出当前脚本的文件名、绝对路径、执行时间戳

shell脚本文件内容为
```shell
#!/bin/bash
echo script name: $(basename $0 .sh)
echo script path: $(cd $(dirname $0); pwd)
echo timestamp:   $(date +%s)
```
![](resources/2023-01-16-22-25-44.png)

## 自定义函数

![](resources/2023-01-16-22-30-52.png)
注意：```[...]```表示这一部分是可有可无的
1. ```function```可以不写
2. 参数列表可以省略（函数的形参不需要定义，默认是```$n```，这一点和脚本是一样的）
3. 返回语句可以省略

![](resources/2023-01-16-22-37-49.png)

shell脚本文件内容为
```shell
#!/bin/bash
function add(){
	s=$[$1 + $2]
	return $s
}
read -p "input first number: " num1
read -p "input second number: " num2
add $num1 $num2
echo "addsum = "$?
```
![](resources/2023-01-16-22-50-21.png)
加法的结果超过255，得到的结果就会出错

解决方法：不用函数的直接return返回，不要用```$?```获取函数返回值，用前面系统函数的调用方式，用命令替换```$()```，把函数返回的值赋给另外一个变量
```shell
#!/bin/bash
function add(){
	s=$[$1 + $2]
	echo $s
}
read -p "input first number: " num1
read -p "input second number: " num2
addsum=$(add $num1 $num2)
echo "addsum = "$addsum
```
![](resources/2023-01-16-22-59-48.png)

## 综合应用

### 归档文件

![](resources/2023-01-16-23-06-40.png)

shell脚本文件内容为
```shell

```




到P82  3min





