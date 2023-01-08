# 生命周期与插件

## 项目构建生命周期

![](resources/2022-12-21-17-00-36.png)

![](resources/2022-12-21-22-39-56.png)

### clean 生命周期

![](resources/2022-12-21-22-41-30.png)

### default 生命周期

![](resources/2022-12-21-22-41-59.png)
比如执行```test```，那就从上到下执行到```test```为止

### site 生命周期

![](resources/2022-12-21-22-42-24.png)

## 插件

![](resources/2022-12-21-22-45-25.png)

在[Maven官网](https://maven.apache.org/)中，可以看到Maven所有的插件，叫做Maven Plugins
![](resources/2022-12-21-22-49-34.png)

### 举例 Apache Maven Source Plugin
![](resources/2022-12-21-22-58-33.png)
其中：
1. 红笔圈出的是插件的坐标
2. 绿笔圈出的是执行到哪一阶段（default生命周期的```generate-test-resources```阶段），执行插件
3. 蓝笔圈出的是执行什么（官网给出了解释，见下图）

![](resources/2022-12-21-23-02-14.png)


到了HM的P15 高级篇  先不学