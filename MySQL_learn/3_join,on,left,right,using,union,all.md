# 多表查询（关联查询）

![](resources/2022-12-09-22-21-29.png)

## 案例

![](resources/2022-12-09-22-22-04.png)

## 出现笛卡尔积的错误

错误的实现方式：
![](resources/2022-12-09-22-39-08.png)
![](resources/2022-12-09-22-29-22.png)
![](resources/2022-12-09-22-29-35.png)
显示的结果是每个员工在每个部门都干过
错误的原因：缺少了多表的连接条件

![](resources/2022-12-09-22-35-06.png)

## 多表查询的正确方式

需要有连接条件

![](resources/2022-12-09-22-40-30.png)
![](resources/2022-12-09-22-43-35.png)

表的别名
![](resources/2022-12-09-22-48-11.png)

n个表至少要有n-1个连接条件
![](resources/2022-12-09-22-51-01.png)

## 多表查询的分类

![](resources/2022-12-09-22-53-25.png)

### 非等值连接

![](resources/2022-12-09-22-55-58.png)

### 自连接

之前的例子都是非自连接

![](resources/2022-12-09-23-04-54.png)
![](resources/2022-12-09-23-05-11.png)

### 内连接 vs 外连接

![](resources/2022-12-09-23-08-05.png)

#### 内连接

![](resources/2022-12-09-23-09-39.png)
员工的数据不全
解决方法：外连接

#### 外连接

![](resources/2022-12-09-23-30-41.png)

![](resources/2022-12-09-23-26-52.png)

![](resources/2022-12-09-23-32-01.png)

JOIN ON 的方式实现多表查询
![](resources/2022-12-09-23-35-45.png)

INNER JOIN 的方式实现内连接，INNER关键字可省略
![](resources/2022-12-09-23-37-42.png)

OUTER JOIN 的方式实现外连接
![](resources/2022-12-09-23-40-00.png)

OUTER 关键字可省略
![](resources/2022-12-09-23-41-01.png)

##### 除了左外连接、右外连接，还有满外连接

![](resources/2022-12-09-23-43-47.png)

![](resources/2022-12-09-23-46-06.png)

![](resources/2022-12-09-23-47-23.png)
后面会重点实现这七种JOIN操作

### 使用 USING 关键字替换连接条件

```sql
SELECT employee_id, last_name, department_name
FROM employees e JOIN departments d
ON e.`department_id` = d.`department_id`;
```

当两个表中的字段名一致的时候，可替换为
```sql
SELECT employee_id, last_name, department_name
FROM employees e JOIN departments d
USING (`department_id`);
```

不适用于自连接

### 自然连接

![](resources/2023-05-22-16-09-37.png)

## 总结

![](resources/2023-05-22-16-22-02.png)

# UNION 组合查询

![](resources/2023-05-22-15-20-54.png)

![](resources/2023-05-22-15-23-32.png)

## 七种图的实现

![](resources/2022-12-09-23-47-23.png)

### 中图：内连接

```sql
SELECT employee_id, department_name
FROM employees e JOIN departments d
ON e.`department_id` = d.`department_id`;
```

### 左上图：左外连接

```sql
SELECT employee_id, department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`;
```

### 右上图：右外连接

```sql
SELECT employee_id, department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`;
```

### 左中图：

```sql
SELECT employee_id, department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL;
```

### 右中图：

```sql
SELECT employee_id, department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL;
```

### 左下图：满外连接

#### 方式1：左上图 UNION ALL 右中图

```sql
SELECT employee_id, department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
UNION ALL
SELECT employee_id, department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL;
```

#### 方式2：左中图 UNION ALL 右上图

```sql
SELECT employee_id, department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
UNION ALL
SELECT employee_id, department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`;
```

### 右下图：

#### 左中图 UNION ALL 右中图

```sql
SELECT employee_id, department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
UNION ALL
SELECT employee_id, department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL;
```

