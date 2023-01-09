# Dockerfile

[Dockerfile菜鸟教程](https://www.runoob.com/docker/docker-dockerfile.html)

Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。

![](resources/2023-01-09-23-16-14.png)

## 举例：centos的Dockerfile

[docker仓库中的centos](https://hub.docker.com/_/centos)

![](resources/2023-01-09-23-20-29.png)

对应的Dockerfile的内容为
```dockerfile
FROM scratch
ADD centos-7-x86_64-docker.tar.xz /

LABEL \
    org.label-schema.schema-version="1.0" \
    org.label-schema.name="CentOS Base Image" \
    org.label-schema.vendor="CentOS" \
    org.label-schema.license="GPLv2" \
    org.label-schema.build-date="20201113" \
    org.opencontainers.image.title="CentOS Base Image" \
    org.opencontainers.image.vendor="CentOS" \
    org.opencontainers.image.licenses="GPL-2.0-only" \
    org.opencontainers.image.created="2020-11-13 00:00:00+00:00"

CMD ["/bin/bash"]
```
其中：
- ```scratch```是所有镜像文件的祖先类
- ```ADD <src> <dest>```该命令将复制指定的```<src>```路径下内容到镜像中的```<dest>```路径下（添加内容到镜像）
- ```LABEL```指令用来给镜像添加一些元数据（metadata），以键值对的形式
- ```/bin/bash```的作用是表示载入容器后运行bash，docker中必须要保持一个进程的运行，要不然整个容器启动后就会马上kill itself，这个```/bin/bash```就表示启动容器后启动bash

## Dockerfile构建过程解析







---

到 P23 尚硅谷