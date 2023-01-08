# 常用五大数据类型

![](resources/2023-01-07-16-16-43.png)

## Redis键（key）

在```redis-cli```中操作

![](resources/2023-01-07-16-06-14.png)

切换数据库
![](resources/2023-01-07-16-03-11.png)

![](resources/2023-01-07-16-21-42.png)

查看、添加key
![](resources/2023-01-07-16-20-32.png)

判断存在、删除key
![](resources/2023-01-07-16-23-59.png)

设置key的过期时间，查看key是否过期
![](resources/2023-01-07-16-26-41.png)

## Redis字符串（String）

![](resources/2023-01-07-16-33-45.png)

set、get命令
![](resources/2023-01-07-16-39-46.png)

![](resources/2023-01-07-16-40-11.png)

append、strlen、setnx命令
![](resources/2023-01-07-16-40-49.png)

![](resources/2023-01-07-16-43-45.png)

incr、decr命令
![](resources/2023-01-07-16-46-04.png)

![](resources/2023-01-07-16-45-43.png)

incrby、decrby命令
![](resources/2023-01-07-16-48-00.png)

mset、mget、msetnx命令
![](resources/2023-01-07-17-23-38.png)

getrange、setrange、setex、getset命令
![](resources/2023-01-07-17-27-02.png)

### 原子性

#### Redis是单线程

![](resources/2023-01-07-16-52-02.png)

![](resources/2023-01-07-16-52-16.png)

#### Java是多线程

Java中的i++操作（能被打断）分为三步：
1. 取值
2. 加1
3. 赋值
![](resources/2023-01-07-16-59-50.png)

i的值的范围是2到200，取到2的极端情况的线程执行次序是：
1. b在第1次**取值**后**加1**前被a打断，此时b取到的i为0
2. a在执行完第99次i++后被b打断
3. b执行第1次**加1**和**赋值**后被a打断，此时i为1
4. a在执行第100次**取值**后**加1**前被b打断，此时a取到的i为1
5. b执行直到第100次i++完成，此时切换回a
6. a执行第100次**加1**和**赋值**，然后i为2，结束

### String底层结构

![](resources/2023-01-07-17-30-15.png)
![](resources/2023-01-07-17-30-55.png)

## Redis列表（List）

![](resources/2023-01-07-17-32-36.png)

lpush、rpush、lpop、rpop、rpoplpush、lrange命令
![](resources/2023-01-07-17-34-35.png)

![](resources/2023-01-07-17-39-57.png)

lindex、llen命令
![](resources/2023-01-07-17-41-38.png)

linsert、lrem、lset命令
![](resources/2023-01-07-17-47-18.png)

![](resources/2023-01-07-17-50-14.png)

![](resources/2023-01-07-17-54-30.png)

### List底层结构

![](resources/2023-01-07-17-56-55.png)

## Redis集合（Set）

![](resources/2023-01-08-15-49-50.png)
![](resources/2023-01-08-15-51-09.png)

sadd、smembers、sismember、scard、srem、spop、srandmember命令
![](resources/2023-01-08-15-52-19.png)

smove、sinter、sunion、sdiff命令
![](resources/2023-01-08-15-55-11.png)

### Set底层结构

![](resources/2023-01-08-15-58-30.png)
![](resources/2023-01-08-15-58-51.png)

## Redis哈希（Hash）

![](resources/2023-01-08-16-01-47.png)
![](resources/2023-01-08-16-06-28.png)
区别：
1. 第一种把数据存成一个String，取值、修改值还要解析，效率比较低
2. 第二种把数据分开存储，如果数据量大，结构会比较混乱
3. 第三种是**key-value套娃**（Redis的Hash的结构）

hset、hget、hmset、hexists、hkeys、hvals、hincrby、hsetnx命令
![](resources/2023-01-08-16-14-15.png)

![](resources/2023-01-08-16-19-25.png)

### Hash底层结构

![](resources/2023-01-08-16-20-55.png)

## Redis有序集合（Zset）

![](resources/2023-01-08-16-22-47.png)

zadd、zrange、zrangebyscore、zrevrangebyscore命令
![](resources/2023-01-08-16-25-24.png)

![](resources/2023-01-08-16-27-57.png)
![](resources/2023-01-08-16-29-02.png)
![](resources/2023-01-08-16-30-05.png)

zincrby、zrem、zcount、zrank命令
![](resources/2023-01-08-16-30-55.png)

### Zset底层结构

![](resources/2023-01-08-16-34-52.png)

#### hash

![](resources/2023-01-08-16-43-47.png)

#### 跳表

![](resources/2023-01-08-16-36-30.png)

![](resources/2023-01-08-16-41-37.png)
![](resources/2023-01-08-16-41-53.png)


