# 02-Docker中的几个基本概念

## 1.镜像

Docker的镜像可以理解为一个包含了os文件系统和应用的对象。如果你使用过一些虚拟化软件比如vmware公司的vspher产品或者是华为的fusioncompute等等，实际上的Docker镜像与这种虚拟化平台所提供或者制作的虚拟机模板是非常类似的。首先我们还是从虚拟机模板讲起，虚拟机的模板是只读的，模板本质是处于未运行状态的虚拟机。在Docker的概念中镜像世界上相当于为运行的容器。如果你有一定的开发经验可以将Docker中的镜像理解为class（只读类），运行中的镜像相当于是一个实例。

​	镜像包含了基础操作系统，以及内部程序所运行所必须的代码和依赖包。Docker中的每个镜像都有自己唯一的ID。用户可以通过引用镜像的ID或者名称使用镜像或者对镜像执行一些列的操作。如果你需要使用镜像通常情况下只需要输入ID开头的几个字符即可——因为ID是唯一的，Docker会自动匹配用户想引用的镜像。

### 镜像相关的操作

1.查看本机上的镜像  

```shell
# docker image ls
```

2.从Dockerhub拉取镜像

```shell
# docker image pull ruby
```

![image-20210906124901159](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906124901159.png)

## 2.容器

前面已经说明了镜像和容器之间的关系，我们可以把镜像类比成虚拟机中的模板，或者开发中的一个只读类。容器就相当于是一个运行中的虚拟机实例或者是类的一个实例。

1.在Linux中启动容器的命令如下

```shell
# docker container run -it ubuntu:latest /bin/bash
```

![image-20210906150133733](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906150133733.png)

docker container run 告诉Docker Daemon启动新的容器。其中-it参数告诉Docker开启容器的交互模式并将当前的shell连接到容器终端。接下来，用户想根据ubuntu:latest镜像创建一个新的容器。如果这个镜像在本地的话docker会使用本地的镜像启动该容器，如果不在本地，docker会自动通过网络下载ubuntu:latest镜像并启动进程。下次在通过ubuntu:latest镜像启动容器就会在本地启动。

2.在容器内运行ps -elf查看当前所在的容器中正在运行中的进程。

![image-20210906150837685](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906150837685.png)

在该容器中有两个进程PID1： /bin/bash进程。这个进程是docker container run 命令来通知容器运行的。

PID10：代表的是ps -elf进程。这个进程实际上在ps -elf退出后这个进程就会消失。也就是说在这个容器中长期的存在的进程只有一个就是/bin/bash进程。
3.从容器中退出，但是不销毁容器。保持容器的正常运行。

​	**使用ctrl + p + q**

4.显示正在运行中的容器

```shell
# docker container ls
```

![image-20210906151950175](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906151950175.png)

5.连接到正在运行中的容器中去

```shell
# docker container exec -it competent_johnson bash
```

![image-20210906152458758](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906152458758.png)

在本次演示中容器实例的名称为competent_johnson这并不是人为创建的而是由docker自动生产的，用户每使用docker创建一个容器进程就会自动为该容器实例创建一个名称。

6.容器的停止

​	现在有一个正在运行中的容器。

![image-20210906153249179](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906153249179.png)

停止容器

```shell
# docker container stop competent_johnson
```

![image-20210906153413263](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906153413263.png)

我在此演示停止容器使用的是容器的名称，你也可以使用docker的ID停止相应容器。在此我不做演示。

7.容器的删除(使用ID)

```shell
# docker container rm 4c8686374687
```

![image-20210906153806272](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906153806272.png)

8.列出所有的容器

​	这一步的目的是确认下镜像已经被完全删除。其中-a参数的作用是实现让Docker列出所有的容器，包括处于停止状态的容器。

```shell
# docker container ls -a
```

