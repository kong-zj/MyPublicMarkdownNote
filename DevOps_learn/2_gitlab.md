# Gitlab 

## 安装

[主页](https://gitlab.com/)
[清华源](https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/ubuntu/pool/bionic/main/g/gitlab-ce/)

依次使用以下命令
```sudo apt-get update```
```sudo apt-get install -y curl openssh-server ca-certificates tzdata perl```
```sudo apt-get install -y postfix```
```wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/ubuntu/pool/bionic/main/g/gitlab-ce/gitlab-ce_12.9.9-ce.0_amd64.deb```
```sudo dpkg -i gitlab-ce_12.9.9-ce.0_amd64.deb```

![](resources/2023-01-02-00-23-49.png)

使用命令```gitlab-ctl --help```，测试是否安装成功
![](resources/2023-01-01-22-08-16.png)

### 修改配置文件

![](resources/2023-01-01-21-44-02.png)

![](resources/2023-01-01-21-41-16.png)

改为本机地址
![](resources/2023-01-01-21-42-41.png)

修改邮件配置

![](resources/2023-01-01-22-13-22.png)

![](resources/2023-01-01-22-15-26.png)
![](resources/2023-01-01-22-16-57.png)

配置结果如下
![](resources/2023-01-01-22-21-32.png)

### 使用```sudo gitlab-ctl reconfigure```重新配置gitlab

![](resources/2023-01-02-00-30-16.png)

但是卡在这里，半天没有反应
![](resources/2023-01-02-00-33-53.png)

![](resources/2023-01-02-00-37-52.png)

最后报错
![](resources/2023-01-02-00-41-55.png)

但是可以访问```172.21.102.63```
![](resources/2023-01-02-00-44-08.png)
注意：这里地址与上面的配置不同，是因为我的WSL子系统每次重启，inet addr会改变，所以要重复上面的操作

## 登录gitlab的Web界面

访问```172.21.102.63```
这里我设置的密码为```12345678```
用默认的用户名```root```和我设置的密码```12345678```登录
![](resources/2023-01-02-00-54-23.png)

登录之后
![](resources/2023-01-02-00-56-35.png)
用法和Github差不多
