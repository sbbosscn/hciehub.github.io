# Docker

## docker是什么？

![image-20210903163929936](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903163929936.png)

Docker是一种能够运行在windows、linux和Mac os上的软件，用于管理创建和编排容器。Docker是在Github上开发的Moby开源项目的一部分。Docker公司，位于旧金山，是整个Moby开源项目的维护者。Docker公司还提供包含支持服务的商业版本的Docker。

![image-20210903164521106](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903164521106.png)

## Docker的安装

### 	windows11平台上的安装。

[docker网站](https://www.docker.com/)

![image-20210903164908723](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903164908723.png)

![image-20210903164940396](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903164940396.png)

选择适合的平台下载相应的安装文件。

![image-20210903165002746](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903165002746.png)

![image-20210903165022361](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903165022361.png)

windows版的docker是一个社区版本（Community Edition，CE）的应用，并不是为生产环境设计的。

本文的安装环境为windows11专业版。这也是笔者第一次尝试安装在windows11上。并未做深入的测试，理论上win10上的安装类似。

![image-20210903165527415](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903165527415.png)

对于在windows平台上安装Docker必须满足一下几点。

- windows必须是64bit
- windows必须支持hyper-v和容器特性
- 电脑硬件平台需要开启虚拟化VT-X

需要确认已在在window平台上开启hyper-v和容器特性。

![image-20210903170029206](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903170029206.png)

![image-20210903170038754](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903170038754.png)

![image-20210903170046229](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903170046229.png)

<img src="https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903170053254.png" alt="image-20210903170053254" style="zoom:33%;" />

<img src="https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903170103848.png" alt="image-20210903170103848" style="zoom:33%;" />

在docker官方网站下载docker安装包。并执行安装运行即可。

![image-20210903170242490](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903170242490.png)

安装成功以后在windows平台上使用终端工具输入

```dockerfile
docker version
```

查看是否安装成功。

![451630660055_.pic](https://gitee.com/hanstack/hanstack_image/raw/master/img/451630660055_.pic.jpg)

### 在linux平台上安装Docker（以Centos为例）

安装docker

```shell
[root@centos-docker ~]# sudo yum install docker-ce docker-ce-cli containerd.io
```

启动docker

```
[root@centos-docker ~]#  sudo systemctl start docker
```

测试docker是否正常安装

```shell
[root@centos-docker ~]# docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started
```

### 在Mac os上安装Docker

下载Docker应用

![image-20210903173201081](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903173201081.png)



![image-20210903173605433](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903173605433.png)

<img src="https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903173624269.png" alt="image-20210903173624269" style="zoom:33%;" />

![image-20210903173938964](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903173938964.png)

![image-20210903174239102](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210903174239102.png)

