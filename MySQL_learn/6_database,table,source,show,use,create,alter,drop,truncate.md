# 数据库（database）和表（table）

## 导入现有数据（source）

***.sql文件
`source [文件的全路径名];`

## SHOW

`show databases;`显示所有数据库

`show tables;`显示某个数据库中的所有表：
![](resources/2022-12-08-15-50-51.png)

`show create database [数据库名];`显示某个数据库的创建信息：
![](resources/2022-12-08-15-57-44.png)

`show create table [表名];`显示某个表的创建信息：
![](resources/2022-12-08-15-49-00.png)

`show columns from [表名];`显示某个表的结构：
![](resources/2022-12-08-15-52-43.png)
上面的语句可简写成`desc jobs;`

如果表名和MySQL保留的关键字冲突，要用着重号（``）括起来
![](resources/2022-12-08-16-16-51.png)

### 查看数据库

![](resources/2024-07-02-21-53-24.png)

## USE

选择数据库

### 切换数据库

![](resources/2024-07-02-21-56-26.png)

## CREATE

创建数据库或创建表

### 创建数据库

![](resources/2024-07-02-21-08-15.png)

### 创建数据表

![](resources/2024-07-02-22-07-53.png)
![](resources/2024-07-02-22-09-38.png)
![](resources/2024-07-02-22-12-29.png)

![](resources/2024-07-02-22-17-32.png)

## ALTER

修改数据库或表

### 修改数据库

![](resources/2024-07-02-22-30-16.png)

### 修改数据表

![](resources/2024-07-02-22-26-33.png)
![](resources/2024-07-02-22-31-52.png)
![](resources/2024-07-02-22-34-22.png)

## DROP

删除数据库或删除表

### 删除数据库

![](resources/2024-07-02-22-30-28.png)

### 删除数据表

![](resources/2024-07-02-22-39-57.png)

## TRUNCATE

### 清空数据表

![](resources/2024-07-02-22-39-57.png)

## DROP、TRUNCATE、DELETE 的区别

DCL 事务简介
![](resources/2024-07-02-22-54-03.png)
![](resources/2024-07-02-23-23-54.png)
其中：
- DROP、TRUNCATE 是 **DDL** 语句
- DELETE 是 **DML** 语句

[MySQL删除表数据、清空表命令（truncate、drop、delete 区别）](https://cloud.tencent.com/developer/article/2389933)















---

之后详细讲
# DCL 中的 COMMIT 和 ROLLBACK







---

# MySQL数据类型

![](resources/2024-07-02-22-01-58.png)
![](resources/2024-07-02-22-02-56.png)














---
到P53





