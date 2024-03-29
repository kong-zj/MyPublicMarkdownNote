# git

## 外部资源

[猴子都能懂的git入门](https://backlog.com/git-tutorial/cn/)
[GitHub漫游指南](https://github.phodal.com/#/chapter/Github%E6%BC%AB%E6%B8%B8%E6%8C%87%E5%8D%97)
[开源指北](https://oschina.gitee.io/opensource-guide/)
[Git官方文档](https://git-scm.com/book/zh/v2)
[GitHub官方文档](https://docs.github.com/cn)

## 开始使用

右键 Git Bash Here
![](resources/2022-11-25-16-30-55.png)

### ```git init``` 命令，用来初始化

![](resources/2022-11-25-16-33-40.png)
目录下的隐藏文件夹 .git
![](resources/2022-11-25-16-34-56.png)

这里暂存区是空的，所以没有```index```文件

![](resources/2022-11-28-23-25-41.png)

### ```git config``` 命令，用来配置git签名

注意这里的签名和登录GitHub的账号、密码没有关系
![](resources/2022-11-25-16-50-20.png)

#### 不同层级

Git 自带一个 git config 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：

1. /etc/gitconfig 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果在执行 git config 时带上 ```--system``` 选项，那么它就会读写该文件中的配置变量。 （由于它是系统配置文件，因此你需要管理员或超级用户权限来修改它。）

2. ~/.gitconfig 或 ~/.config/git/config 文件：只针对当前用户。 你可以传递 ```--global``` 选项让 Git 读写此文件，这会对你系统上 所有 的仓库生效。
![](resources/2022-11-25-16-56-52.png)

3. 当前使用仓库的 Git 目录中的 config 文件（即 .git/config）：针对该仓库。 你可以传递 ```--local``` 选项让 Git 强制读写此文件，虽然默认情况下用的就是它。 （当然，你需要进入某个 Git 仓库中才能让该选项生效。）
![](resources/2022-11-25-16-55-26.png)

每一个级别会覆盖上一级别的配置（局部优先），所以 .git/config 的配置变量会覆盖 /etc/gitconfig 中的配置变量。

## 结合 Github 使用 git

首先在 Github 中创建一个仓库
![](resources/2023-12-04-10-35-46.png)

![](resources/2023-12-04-13-29-21.png)

克隆这个仓库
![](resources/2023-12-04-13-30-34.png)
就可以使用了

## Github 被墙了（挂VPN）

### VPN 打开网卡级别加速模式

![](resources/2022-12-07-00-52-53.png)
平常浏览网页时，不要开此模式，能省流量

ping谷歌测试一下
![](resources/2022-12-07-00-54-50.png)

但是此时```git push```报错
![](resources/2022-12-07-00-55-27.png)

解决方案：
![](resources/2022-12-07-00-56-25.png)

### 配置 git 的代理

![](resources/2022-12-07-00-57-53.png)

![](resources/2022-12-07-00-58-24.png)
此时```git push```成功

但是这种情况下使用ssh协议则不成功：
![](resources/2022-12-07-01-03-44.png)
算了，先用https协议

## 忽略文件

![](resources/2022-12-07-21-51-19.png)

![](resources/2022-12-07-22-02-35.png)
[网址链接](https://github.com/github/gitignore)

举例
![](resources/2022-12-07-21-58-19.png)

![](resources/2022-12-07-22-13-46.png)

```.gitignore```文件一般放在项目的根目录中

配置忽略文件的路径
![](resources/2022-12-07-22-21-47.png)

从Github上找对应语言的忽略文件，放入项目根目录
![](resources/2022-12-07-22-23-54.png)

提交即可
![](resources/2022-12-07-22-28-17.png)

![](resources/2022-12-07-22-29-47.png)