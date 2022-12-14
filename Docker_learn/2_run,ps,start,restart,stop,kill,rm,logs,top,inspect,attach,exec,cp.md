# 容器命令

![](resources/2022-12-15-14-00-32.png)

![](resources/2022-12-15-14-02-04.png)

## run

新建并启动容器
![](resources/2022-12-15-14-03-58.png)
![](resources/2022-12-15-14-03-23.png)

### docker run -it 5d0da3dc9764

启动交互模式并进入终端
![](resources/2022-12-15-14-11-53.png)

### docker run -d centos

后台运行容器
![](resources/2022-12-15-14-58-03.png)
上图可见，用```docker ps```命令找不到它
![](resources/2022-12-15-15-00-52.png)
类似于一个饭店，长时间没有客人，就把灶火停了

让它有事情可做，实例见下方的logs里

## ps

类似于linux中的命令```ps -ef```

列出所有正在运行的容器
![](resources/2022-12-15-14-15-29.png)

![](resources/2022-12-15-14-13-56.png)

![](resources/2022-12-15-14-14-45.png)
实例见下方退出容器的exit里

## 退出容器

![](resources/2022-12-15-14-16-34.png)

### exit

容器**停止**退出
![](resources/2022-12-15-14-17-41.png)
![](resources/2022-12-15-14-17-59.png)

用```docker ps```命令的效果：
![](resources/2022-12-15-14-21-08.png)

### Ctrl + P + Q

先把之前停止退出的centos启动：
![](resources/2022-12-15-14-24-33.png)

容器**不停止**退出
![](resources/2022-12-15-14-31-09.png)
![](resources/2022-12-15-14-32-10.png)

如果之后想再进入这个容器，就用```docker attach```命令

## start

启动容器
![](resources/2022-12-15-14-35-28.png)
容器可以停止，也可以启动

## restart

重启容器
![](resources/2022-12-15-14-38-27.png)

## stop

温柔停止容器
![](resources/2022-12-15-14-40-52.png)
![](resources/2022-12-15-14-40-38.png)

## kill

强制停止容器
![](resources/2022-12-15-14-42-30.png)
![](resources/2022-12-15-14-41-54.png)

## rm

注意：
1. ```rmi```是删除镜像（image）
2. ```rm```是删除容器（container）

删除已停止的容器
![](resources/2022-12-15-14-42-49.png)
![](resources/2022-12-15-14-46-10.png)

对于没有停止的容器，用参数```-f```可以强制删除

删除全部的方法：
![](resources/2022-12-15-14-48-22.png)
上图中的命令结构类似sql查询，管道前查出的结果作为参数交给管道后的

## logs

查看日志
![](resources/2022-12-15-15-06-15.png)

让后台运行的容器有事情可做
![](resources/2022-12-15-15-10-58.png)
![](resources/2022-12-15-15-13-20.png)

## top

类似于linux中的命令```top```

查看容器内运行的进程
![](resources/2022-12-15-15-19-19.png)

大部分的linux命令在docker中都可以使用

## inspect

因为docker镜像像同心圆一样，一层套一层

查看容器内部的细节
![](resources/2022-12-15-15-24-20.png)
返回JSON串

## attach

![](resources/2022-12-15-15-34-58.png)

进入正在运行的容器
![](resources/2022-12-15-15-33-49.png)

## exec

功能比attach强大

![](resources/2022-12-15-15-34-58.png)

隔山打牛，在外面对容器进行远程操作，并把结果返回到外面
![](resources/2022-12-15-15-38-14.png)

进去再干活
![](resources/2022-12-15-15-42-53.png)

## cp

容器内外之间的拷贝
![](resources/2022-12-15-15-49-50.png)

![](resources/2022-12-15-15-50-18.png)


