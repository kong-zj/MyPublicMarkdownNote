# 后悔药

![](resources/2022-12-05-22-41-13.png)

## ```git restore```

### ```git restore [文件名]```，撤回工作区中的修改（方法1）（用暂存区中的数据覆盖）

![](resources/2022-12-05-22-46-29.png)

![](resources/2022-12-06-15-09-40.png)
注意：
1. 使用**暂存区**中的文件数据覆盖工作区中的文件数据
2. 而不是使用**版本库**中的文件数据覆盖工作区中的文件数据

#### ```git checkout [HEAD] [文件名]```命令的作用类似但不同（方法2）（用版本库中的数据覆盖）

![](resources/2022-12-06-15-05-23.png)
注意：
1. 使用**版本库**中的文件数据覆盖工作区中的文件数据
2. 而不是使用**暂存区**中的文件数据覆盖工作区中的文件数据

注意```git checkout [文件名]```和```git checkout HEAD [文件名]```命令的不同：
影响是否也用**版本库**中的文件数据覆盖**暂存区**中的文件数据，见下图
![](resources/2022-12-06-15-35-02.png)

注意```git checkout```的两种用法：
1. +分支名
2. +文件名
   
具体的不同，见```git reset```下的```git checkout```

### ```git restore --staged [文件名]```，撤回暂存区中的修改（方法1）

对文件```a.txt```的修改已经放入**暂存区**
![](resources/2022-12-05-22-52-20.png)

![](resources/2022-12-05-22-55-53.png)

![](resources/2022-12-05-22-57-04.png)

此时，**暂存区**的修改已经撤回，如果要撤回**工作区**的修改，再依次操作

## ```git commit```

### ```git commit --amend```，撤回版本库中的修改（通过新的提交覆盖上一次的提交）

```git commit --amend```命令把新的内容添加到之前的commit里面,这个命令没有添加新的提交，而是用新提交取代了原始提交

情况1：
提交日志写错了，导致日志不干净
![](resources/2022-12-05-23-13-32.png)

![](resources/2022-12-05-23-18-21.png)

使用```git commit --amend```命令
![](resources/2022-12-05-23-22-03.png)

将写错的日志修改到正确
![](resources/2022-12-05-23-21-29.png)

情况2.1：
文件被暂存后，又被修改了，但是忘了暂存，就提交了
![](resources/2022-12-05-23-30-16.png)

![](resources/2022-12-05-23-32-52.png)

发现自动把**未暂存的修改**暂存了
![](resources/2022-12-05-23-35-38.png)

这里重新测试了一遍，发现确实是这样
![](resources/2022-12-05-23-45-27.png)

那这样提交的结果是符合要求的，就不要重新提交了

情况2.2：
提交的文件内容有误，那就修改完成后重新提交
![](resources/2022-12-06-10-21-04.png)

![](resources/2022-12-06-10-16-01.png)

发现文件的内容写错，但是已经提交
![](resources/2022-12-06-10-16-59.png)

修改文件内容，用参数```--amend```重新提交，会覆盖最近一次的提交
![](resources/2022-12-06-10-19-19.png)

情况1和情况2实际是一回事，别想复杂了

情况3：
提交的内容完全没有用处，想回到之前的提交版本
![](resources/2022-12-06-10-22-40.png)

## ```git reset```

### ```git reset [--mixed]/--hard/--soft HEAD^/[commit对象的Hash值]```，回到指定的commit版本

下面为本例：
![](resources/2022-12-06-10-29-40.png)

当前状态为：
![](resources/2022-12-06-10-35-27.png)

#### 使用参数```--soft```

本质是撤销上次的```git commit```命令
相当于如果不用```git reset --soft HEAD^```命令的话，可以在下次提交时，用```git commit --amend```代替```git commit```（```soft```+```commit```=```amend```）
只动**版本库**
![](resources/2022-12-06-10-41-27.png)

下图展示了使用**底层命令**来查看**版本库、暂存区、工作区中的对应文件**
![](resources/2022-12-06-10-54-34.png)
可见，只动了版本库，没有动暂存区、工作区

为了接下来的实验，还需要回到初始状态：

![](resources/2022-12-06-11-03-58.png)
注意```git log```和```git reflog```的区别：
1. ```git log```只会显示递增提交的版本信息
2. ```git reflog```只要移动HEAD都会记录（用于数据恢复）

#### 使用参数```[--mixed]```

这里的参数加了中括号，代表这是默认值，可以省略不写
本质是撤销上次的```git commit```和```git add```命令
只动**版本库**、**暂存区**
![](resources/2022-12-06-10-59-48.png)

![](resources/2022-12-06-11-13-36.png)
可见，只动了版本库、暂存区，没有动工作区

回到初始状态：

![](resources/2022-12-06-11-20-23.png)

#### 使用参数```--hard```

本质是撤销上次的```git commit```和```git add```命令以及工作目录中的所有操作
只动**版本库**、**暂存区**、**工作区**（类似```git checkout [分支名]```也动这三个区域，区别只有HEAD是否带分支名一起移动）
![](resources/2022-12-06-13-36-24.png)

![](resources/2022-12-06-13-40-10.png)
可见，只动了版本库、暂存区、工作区

注意```--hard```的危险
![](resources/2022-12-06-13-42-21.png)

### ```git reset [--mixed] [HEAD] [文件名]```（路径reset），在暂存区把指定的文件撤回到上个版本（撤销暂存区的修改）（方法2）

这里的中括号```[]```的表达意义略有冲突：
1. ```[HEAD]```表示可省略
2. ```[文件名]```表示一个整体，是让中文在英文环境中看着舒服，不是省略的意思

只有用参数```--mixed```才支持路径reset
如果后面不跟文件名，表示撤回所有文件

下图中的跳过第一步指的是：**不动版本库**，**只动暂存区**
因为参数是```--mixed```，所以**不动工作区**
![](resources/2022-12-06-14-02-53.png)

下图可见，```git reset [文件名]```命令只动暂存区，不动工作区
![](resources/2022-12-06-15-22-24.png)

当修改了某文件，并加入到暂存区，但是还没有提交
```git reset [--mixed] [HEAD] [文件名]```中的```[HEAD]```代表上一次的提交内容，即上一次暂存区中的内容，这个命令相当于拿上一次的暂存区覆盖当前的暂存区

![](resources/2022-12-06-14-04-36.png)

![](resources/2022-12-06-14-04-57.png)

### ```git reset```和```git checkout```都有加{分支名/commit对象的Hash值}和{文件名}这两种用法

#### 与```git checkout [分支名]```相比：
![](resources/2022-12-06-22-49-57.png)
1. ```git reset [commit对象的Hash值]```是HEAD带着分支**一起**移动，而```git checkout [分支名]```是HEAD自己**单独**移动
2. 用```git reset --hard [commit对象的Hash值]```会**强制覆盖**工作区，而```git checkout [分支名]```对工作区是**安全**的
3. ![](resources/2022-12-06-14-17-45.png)
4. ![](resources/2022-12-06-14-19-26.png)
5. 所以说，```git checkout [分支名]```的底层命令就是```git reset --hard [commit对象的Hash值]```，在此基础上有改变：
   1. 分支没有跟着一起动（分支保存的commit对象的Hash值不变）
   2. 覆盖工作目录时没有直接覆盖，会判断工作目录是否有新增的文件，新增的文件会保留

#### 不仅有```git checkout [分支名]```的用法，还有```git checkout [文件名]```的用法：
下图中的跳过第一步指的是：**不动版本库**，**动暂存区**，**动工作区**
下图中的跳过第一步和第二步指的是：**不动版本库**，**不动暂存区**，**只动工作区**
注意：是否动**暂存区**，由是否加参数```HEAD```决定，详见见```git restore```下的```git checkout```
注意下图中的reset指的是```git reset [--mixed] [HEAD] [文件名]```，而不是```git reset [commit对象的Hash值]```，所以说不移动HEAD是对的
![](resources/2022-12-06-14-26-16.png)

可以说，```git checkout [文件名]```的底层命令就是```git reset --hard [HEAD] [文件名]```，但是这个底层命令是臆想的，方便统一理解，不能执行
![](resources/2022-12-06-23-24-31.png)

注意```git checkout [文件名]```和```git checkout HEAD [文件名]```又不同：
![](resources/2022-12-06-23-31-24.png)

总结：
![](resources/2022-12-07-15-09-49.png)

对应关系
![](resources/2022-12-07-15-17-38.png)

![](resources/2022-12-07-15-14-07.png)

![](resources/2022-12-07-15-19-46.png)
