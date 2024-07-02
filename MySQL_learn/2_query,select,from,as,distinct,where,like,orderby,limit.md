# 初步SELECT（query）

## select ... from ...

`select [字段1,字段2, ... ] from [表名];`查询某个表的某些列的数据：
![](resources/2022-12-08-15-59-20.png)

## as 列的别名（alias）

`select [全称] as [别名] from [表名];`
其中关键字as可省略
![](resources/2022-12-08-16-06-50.png)
如果别名中有空格，要用双引号包起来，不要用单引号

## distinct 去除重复行

必须放在所有列名的最前面
![](resources/2022-12-08-16-19-03.png)

### 正确用法

![](resources/2022-12-08-16-22-59.png)

### 错误用法

不能部分使用
![](resources/2022-12-08-16-21-24.png)

## 空值null参与运算

null不等同于0

计算结果为null，因为null参与了计算
![](resources/2022-12-08-16-29-45.png)

### ifnull

如果想把null当作0参与运算
![](resources/2022-12-08-16-32-12.png)

## where 过滤数据

`select [字段] from [表名] where [过滤条件];`
![](resources/2022-12-08-16-43-45.png)

### 运算符

#### 算术运算符

![](resources/2022-12-08-22-08-14.png)

注意，加法在MySQL中没有连接的作用
这里字符串隐式转换成数值
![](resources/2022-12-08-22-11-16.png)

字符串不能转换成数值，就按0处理
![](resources/2022-12-08-22-12-12.png)

#### 比较运算符

![](resources/2022-12-08-22-16-35.png)

##### = 等于号

![](resources/2022-12-08-22-19-54.png)

两个字符串之间比较，就不会隐式转换成数值
![](resources/2022-12-08-22-20-37.png)

只要有NULL参与，结果就是NULL
![](resources/2022-12-08-22-23-06.png)

![](resources/2022-12-08-22-26-44.png)
没有达到预期效果，引出 <=> 安全等与号

##### <=> 安全等于号

为NULL而生
![](resources/2022-12-08-22-29-32.png)

![](resources/2022-12-08-22-30-22.png)

![](resources/2022-12-08-22-31-16.png)
达到预期效果

也可以用关键字ISNULL判断
![](resources/2022-12-08-22-33-29.png)

![](resources/2022-12-08-22-34-25.png)
效果相同

![](resources/2022-12-08-22-36-06.png)
还是效果相同

查找不是NULL的四种方法
![](resources/2022-12-08-22-38-12.png)

##### like 模糊查询

![](resources/2022-12-08-22-49-42.png)

![](resources/2022-12-08-22-50-47.png)

![](resources/2022-12-08-22-53-51.png)

使用关键字ESCAPE可以指定某字符为转义字符

##### REGEXP / RLIKE 正则表达式

![](resources/2022-12-08-22-58-54.png)

#### 逻辑运算符

![](resources/2022-12-08-22-59-57.png)

#### 位运算符

![](resources/2022-12-08-23-02-04.png)

## order by 排序数据

![](resources/2022-12-08-23-42-37.png)
ASC:ascend
DESC:descend

![](resources/2022-12-08-23-44-06.png)

### 用列的别名排序

![](resources/2022-12-08-23-48-13.png)

#### 因为涉及到语句执行顺序问题：
1. 先执行 **FROM**、**WHERE** 选出符合条件的行
2. 在从这些行中 **SELECT** 相应的列数据
3. 最后再用 **ORDER BY** 排序

别名只能在SELECT之后的语句（ORDER BY）中才能使用，之前的语句（WHERE）执行时还没有这个别名

![](resources/2022-12-08-23-53-29.png)

WHERE要声明在FROM之后，ORDER BY之前

### 二级排序

![](resources/2022-12-09-00-01-31.png)

## limit 分页

类似切片
![](resources/2022-12-09-00-05-28.png)

![](resources/2022-12-09-00-06-36.png)

limit的位置
![](resources/2022-12-09-00-09-00.png)

![](resources/2022-12-09-00-11-14.png)

总结：
![](resources/2022-12-09-00-19-31.png)
