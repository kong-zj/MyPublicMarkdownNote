# 子查询（subquery）

## 简介

![](resources/2024-07-01-21-12-58.png)

## 分类

![](resources/2024-07-01-21-15-22.png)

![](resources/2024-07-01-21-18-05.png)

## 单行子查询

![](resources/2024-07-01-21-19-48.png)

### 子查询可以出现的位置

![](resources/2024-07-01-23-27-00.png)

### WHERE 中的子查询

![](resources/2024-07-01-22-03-13.png)
![](resources/2024-07-01-22-04-52.png)

### HAVING 中的子查询

![](resources/2024-07-01-22-10-44.png)

### CASE 中的子查询

![](resources/2024-07-01-22-31-14.png)

### FROM 中的子查询

![](resources/2024-07-01-23-19-09.png)

### ORDER BY 中的子查询

![](resources/2024-07-01-23-24-31.png)

## 多行子查询（集合比较子查询）

![](resources/2024-07-01-22-39-46.png)

### IN 操作符

![](resources/2024-07-01-22-43-10.png)

### ANY 操作符

![](resources/2024-07-01-22-47-16.png)

### ALL 操作符

![](resources/2024-07-01-22-48-10.png)

## 相关子查询（关联子查询）

前面的案例，基本都是不相关子查询，但不是说单行子查询和多行子查询都是不相关的，只是从不同的角度来分类

### 执行流程

![](resources/2024-07-01-23-09-17.png)

![](resources/2024-07-01-23-15-29.png)
![](resources/2024-07-01-23-19-09.png)

![](resources/2024-07-01-23-32-58.png)

### EXISTS 与 NOT EXISTS 关键字

![](resources/2024-07-02-13-55-14.png)

![](resources/2024-07-02-14-01-48.png)

![](resources/2024-07-02-14-02-18.png)

### 相关更新

![](resources/2024-07-02-14-24-33.png)

### 相关删除

![](resources/2024-07-02-14-26-10.png)

### 效率问题

![](resources/2024-07-02-14-28-32.png)
