# 04-Docker容器

## 1.容器简介

容器是镜像运行时的实例。如同从虚拟化平台上通过模板的方式启动虚拟机是一样的。用户可以从单个镜像启动一个或者多个容器。

​	容器是打包代码及其所有依赖项的标准软件单元，因此可以方便程序在不同的计算环境之间实现快速，可靠的迁移。Docker Container Image是一个独立，标准化，轻量级，可执行的软件包。其中包含运行应用程序所需的一切：代码、运行环境、系统工具、系统库和配置文件。

​	虚拟机和容器之间的最大区别是容器更加快速并且更加轻量级——虚拟机需要运行在完整的操作系统docker只需要与宿主机的操作系统共享内核。

![img](https://gitee.com/hanstack/hanstack_image/raw/master/img/container-what-is-container.png)

## 2.容器详解

容器和虚拟机之间的比较。在前面的内容讲解过程中一直用虚拟机跟容器类比的方式帮助大家理解什么容器。但是容器和虚拟机之间还是有些许的区别的，并且这也很重要。

​	容器和虚拟机都依赖于宿主机才能正常运行。宿主机可以使笔记本，数据中心的物理服务器，也可以是在云上的弹性云服务器实例。

​	通常在虚拟机模型中，首先要开启物理机启动hypervisor引导程序。一旦hypervisor启动，就会占用机器上全部的物理资源。包括计算资源，存储资源和网络资源。然后hypervisor接管这些资源之后会把这些资源划分到相应的资源池当中形成颗粒度较小的虚拟资源。把不同的虚拟资源按照所需的配置的打包进虚拟机中，这样用户就能通过虚拟机去调度这些虚拟资源并在上安装操作系统和应用提供给用户使用。

![2411631001011_.pic_hd](https://gitee.com/hanstack/hanstack_image/raw/master/img/2411631001011_.pic_hd.jpg)

​	容器的模型与虚拟机模型相比有些许不同。

​	服务器启动后，服务器上运行的操作系统启动。在Docker世界中可以选择linux，或者内核支持内核中的容器原语的新版本的windows。与虚拟机模型相同。OS也占用了全部硬件资源。在OS层之上，需要安装容器引擎（如Docker）容器可以获取系统资源，比如进程树、文件系统以及网络栈，接着讲资源分割成为安全的相互隔离的资源结构，称之为容器。每个容器看起来就像一个真实的操作系统，在其内部可以运行应用。

![image-20210907191357805](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907191357805.png)

​	Hypervisor实现的硬件虚拟化，Hypervisor将硬件资源划分为虚拟化资源；另外，容器是操作系统虚拟化。多个容器共享宿主机的内核。

​	虚拟机带来的开销问题。虚拟机模型将底层的硬件资源划分到虚拟机当中。每个虚拟机都分配了一定的CPU，内存，存储，网络等资源。然后在虚拟机上安装操作系统，然后在虚拟机上安装所需的应用软件，但是虚拟机上的操作系统会带来开销，也就是说虚拟机上的操作系统会占用一定的资源来维持自身的正常运行和对虚拟机上硬件和软件资源的管理。通常这种开销被称之为OS Tax或者是VM Tax。

​	容器模型具有在宿主机操作系统中运行的单个内核。在一台宿主机上运行数十个或者数百个容器也是可行的——在宿主机操作系统上的容器共用一个内核。这意味着只有一个操作系统消耗cpu，内存，存储资源。只有一个操作系统需要授权license。也就只有一个操作系统消耗资源，这样一来大大减少了资源的开销。

​	容器和虚拟机相比并不是一个完整意义上的操作系统。容器的启动时间是要远远快于传统的虚拟机的。**容器内部并不包含内核**。

​	linux的操作系统由内核空间和用户空间组成，内核空间是kernel，用户空间是rootfs, 不同Linux发行版的区别主要是rootfs.不同的容器共用一个内核只是rootfs不同。容器不需要内核因为内核由宿主机上的操作系统提供。

![2421631018068_.pic](https://gitee.com/hanstack/hanstack_image/raw/master/img/2421631018068_.pic.jpg)

![image-20210907203503510](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907203503510.png)

在这里我可以做一个实验佐证我刚才的说法。我搭建了两套docker实验环境，实验平台A使用的centos 7操作系统作为宿主机的操作系统；安装最新版本的docker环境。实验平台B使用的centos 8操作系统作为宿主机的操作系统；同样安装最新版本的docker环境。实验平台A和实验平台B都通过docker pull ubuntu:latest拉取官方仓库的默认ubuntu镜像。分别查看两套平台中运行的ubuntu容器中内核的版本和宿主机的版本。

​	centos 8

![image-20210907210724592](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907210724592.png)

​	centos 7

![image-20210907210845943](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907210845943.png)

## 3.检查docker Daemo

通常登录Docker主机的第一件事就是检查Docker是否在运行状态。

```shell
# docker version
```

```
[root@Docker ~]# docker version
Client: Docker Engine - Community
 Version:           20.10.8
 API version:       1.41
 Go version:        go1.16.6
 Git commit:        3967b7d
 Built:             Fri Jul 30 19:55:49 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.8
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.6
  Git commit:       75249d8
  Built:            Fri Jul 30 19:54:13 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.9
  GitCommit:        e25210fe30a0a703442421b0f60afac609f950a3
 runc:
  Version:          1.0.1
  GitCommit:        v1.0.1-0-g4144b63
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

当输出的内容包含Client和Server的内容时，表示Docker Daemon正在运行。

如果Docker Daemon没有处在运行状态可以使用

```shell
# service docker status
```

启动docker daemon，只要显示如下内容表示启动成功。

![image-20210907212221584](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907212221584.png)

## 4.启动容器

```shell
# docker container run -it ubuntu:latest /bin/bash
```

如果本地有相关镜像就直接将镜像启动为容器。

![image-20210907212447696](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907212447696.png)

如果要启动的容器的镜像不在本地，那么启动镜像需要先从网络上的仓库进行下载。下载完成后才能运行。

```shell
# docker container run -it node 
```

![image-20210907213004735](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907213004735.png)

## 5.容器进程

在启动ubuntu镜像运行Bash shell（/bin/bash）。这使得**bash shell成为该容器中唯一运行的进程**，但是使用ps -elf查看进程是发现ubuntu容器中实际上包含两个进程。列表中PID为1 的是容器中运行的bash shell进程表，而PID 10是ps -elf产生的，这是一个临时进程，在输出结果后该进程就退出了。

![image-20210908093541710](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908093541710.png)

## 6.容器的生命周期

​	所谓的生命周期实际上可以理解为容器从创建到销毁的过程中做的所有操作都可以称之为容器的生命周期。

​	一个比较重要的问题，容器能够对数据进行持久化操作吗？**容器是可以对数据进行持久化操作的**。

​	本次实验会创建一个新的容器，用来观察该容器完整的生命周期。

1.启动一个ubuntu:latest容器并将其命名为percy

```shell
# docker container run --name percy -it /bin/bash
```

2.将数据写入到tmp目录下的某个文件

```shell
root@eea152e1190b:/# cd tmp/
root@eea152e1190b:/tmp# ll
total 0
drwxrwxrwt. 2 root root 6 Aug 27 07:27 ./
drwxr-xr-x. 1 root root 6 Sep  8 01:49 ../
root@eea152e1190b:/tmp# echo "Miaomiao" > myheart
root@eea152e1190b:/tmp# ls -l
total 4
-rw-r--r--. 1 root root 9 Sep  8 01:55 myheart
root@eea152e1190b:/tmp# cat myheart
Miaomiao
```

![image-20210908095550058](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908095550058.png)

3.接下来停止运行percy容器

```shell
# docker container stop percy
```

![image-20210908100507540](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908100507540.png)

4.查看percy容器是否还在运行状态。

```shell
# docker container ls 
```

正在运行中的容器中已经没有名称为percy的容器了，证明该容器已经停止运行。

![image-20210908100808243](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908100808243.png)

5.查看所有容器的状态包括处于停止状态的容器

```shell
# docker container ls -a
```

![image-20210908101827312](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908101827312.png)

6.重启percy容器

```shell
# docker container start percy
```

![image-20210908101925655](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908101925655.png)

7.查看percy容器的是否被启动

![image-20210908102016159](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908102016159.png)

8.连接到percy容器

```shell
# docker container exec -it percy /bin/bash
```

![image-20210908102202155](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908102202155.png)

9.查看percy容器中创建的文件是否存在

```shell
root@eea152e1190b:/# cd tmp/
root@eea152e1190b:/tmp# ll
total 4
drwxrwxrwt. 1 root root 21 Sep  8 01:55 ./
drwxr-xr-x. 1 root root 29 Sep  8 01:49 ../
-rw-r--r--. 1 root root  9 Sep  8 01:55 myheart
root@eea152e1190b:/tmp# cat myheart
Miaomiao
```

 容器重启后，之前在容器中创建的文件依然存在，并且文件中包含的数据并没有发生变更。这说明容器重启或者停止运行都不会损毁容器和其中的数据。

​	上面的实验说明了容器是能够进行数据的持久化存储的，但是实际上卷（volume）才是容器中存储持久化数据的首选方式。

直接删除运行中的容器。

​	首先查看正在运行中的容器

```shell
# docker container ls 
```

​	![image-20210908103756418](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908103756418.png)

删除容器可以使用Container ID或者是容器的名称（名称可以自动生成也可以人为指定）

```shell
# docker container rm percy  cb9162562fbe -f
```

![image-20210908104118157](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908104118157.png)

虽然使用 -f参数可以直接不加提示的产出运行中的容器；但是非常不建议这么做。在生产活动中删除容器最好还是分两步：1.停止正在运行中的容器 2.删除停止的容器。这样才足够优雅。

​	容器的生命周期；包含了容器的启动，运行，停止，暂停等多次操作。除非删除容器，否则容器并不会丢失其中的数据。但是如果数据是存放在卷当中的，即使删除卷，数据也会被保存下来。

## 7.容器的重启策略

​	在运行容器时最好配置好容器的重启策略。这是容器的一种自我修复。可以在指定时间或者发生错误后通过重启来完成自我修复（重启真的能够解决百分之九十九的问题 哈哈）。

​	重启策略可以应用于每个容器，可以作为参数传入docker-container run命令中或者早Compose文件中声明（需要在使用Docker Compose以及Docker Stacks时才可以）。

​	容器支持的重启策略。

- always

  always是一种简单的重启策略；除非使用手动停止；如docker container stop [containerID|containerName]的方式停止运行的容器。always还有一个有意思的地方在于当docker daemon重启的时候，即使手动方式停止的容器也会重启。

- unless-stopped

  unless-stoppde和always最大的区别就是当docker daemon重启的时候；容器的重启策略为unless-stopped时不会被策略触发重启操作。

- on-failed

  on-failed策略会在退出容器并且返回值不是0（0表示正常退出）的时候，触发容器的重启策略。就算容器处于stopped状态并且在Docker deamon重启的时候，容器也会被重启。

- on-failed:3

  与on-failed的策略基本一致，只不过最多重启3次。

- no（default）

  默认策略，在容器退出时不重启容器。

### always

```shell
# docker container run --name always -it --restart always ubuntu:latest /bin/bash
```

用命令查看运行中的容器发现CREATE时间是4minutes之前，但是启动时间是11seconds之前。表示在执行exit退出后，docker显示自动杀死了这个容器进程然后出发了容器的always策略自动将容器重启了。

```shell
# docker container ls 
```



![image-20210908141851218](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908141851218.png)

在always策略的容器停止的情况下，重启docker deamon；发现always策略的容器自动重启了。

```shell
# docker container stop always 
# docker container ls -a
# systemctl restart docker 
# docker container ls 
```

![image-20210908143157410](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908143157410.png)

### unless-stopped

创建一个重启策略为unless-stopped的容器

```shell
# docker container run --name unless-stopped -it --restart unless-stopped ubuntu:latest /bin/bash
```

![image-20210908144144021](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908144144021.png)退出容器之后；查看该容器的策略是否执行。显示容器的创建时间是22秒前，但是创建时间是7秒前。表示出发了unless-stopped策略，将容器重启了。

![image-20210908144205240](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908144205240.png)

使用命令手动停止运行中的容器。

```shell
# docker container stop unless-stopped
```

![image-20210908144532920](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908144532920.png)

查看容器的状态

```shell
# docker container ls -a
```

![image-20210908144559016](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908144559016.png)

重启docker deamon

```
# systemctl restart docker 
```

![image-20210908144630355](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908144630355.png)

在此查看该容器的状态发现容器没有被启动

![image-20210908144852268](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908144852268.png)

## 8.快速清理所有的docker容器

```shell
# docker container rm $(docker container ls -aq) -f
```

## 9.查看容器的配置细节和运行时信息。

可以使用容器ID或者容器名

```shell
# docker container inspect ubuntutest
```

![image-20210908173424947](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210908173424947.png)

## 10.导出容器

如果要导出本地某个容器，可以使用docker export命令

​	查看容器的ID或者名称

```shell
# docker container ls 
```

![image-20210909001104061](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210909001104061.png)

​	导出容器

```shell
# docker container export 9ecb95ba5b7e > nginx.tar
```

![image-20210909001335283](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210909001335283.png)

## 11.导入容器

如果要导入本地某个容器，可以使用docker import命令

```shell
# docker import xxx.tar <image:Ltag>
```



## 12.文件拷贝



## 13.目录挂载（容器数据卷操作）

​	在创建容器的时候，将宿主机的目录与容器内的目录进行映射，这样就可以通过修改宿主机某个目录的文件从而去影响容器，而且这个操作是双向绑定的，也就是说容器内的操作也会影响到宿主机实现备份功能。

​	但是容器被删除的时候，宿主机内的内容不会被删除。如果多个容器挂载同一个目录，其中一个容器被删除，其它容器的内容也不会受到影响。
​	容器与宿主机之间的数据属于引用关系，数据卷是从外界挂载到容器内部的，所以可以脱离容器的生命周期而独立存在，正是由于数据卷的生命周期不等于容器的生命周期，在容器退出或者删除以后，数据仍然不会受到影响，数据卷的生命周期会一直持续到没有容器使用它为止。

​	创建容器添加 -v参数，格式为 ```宿主机目录：容器目录```,例如：

​	将宿主机的一个一个目录/dockervolume挂载给新创建的容器centos7_01的/usr/local/data_volume。

### 指定目录挂载

1.在宿主机创建挂载目录dockervolume;并在其中创建dockervolume.txt文件。

```shell
# mkdir /dockervolume
# cd /dockervolume
# vi dockervolume.txt
```

![image-20210914164947654](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210914164947654.png)

```shell
#docker container run  -di -v /dockervolume:/usr/local/data_volume --name centos7_01 centos:7
```

![image-20210914165223248](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210914165223248.png)

2.在容器内查看是否有挂载目录；内容是否与宿主机一致

```shell
# cd /usr/local/data_volume
# ls
# cat dockervolume.txtcen
```

![image-20210914165604040](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210914165604040.png)

多目录挂载：

​	同时将多个目录挂载到同一个容器。

```shell
# docker container run -di -v /宿主机目录:/容器目录 -v /宿主机目录2:/容器目录 镜像名
```

目录挂载操作可能会出现权限不足的提示，这是因为centos7中的安全模块SELinux吧权限禁掉了，在docker run时通过 --privilege=true给该容器增加权限来解决权限不足的问题。

### 	匿名挂载

​	匿名挂载只需要写容器目录即可，容器外对应的目录会在/var/lib/docker/volume中生成。

```shell
# docker  run -di -v /usr/local/niming --name niming  centos:7
```

​	使用docker inspect <ContainerName/Container ID>能够找到容器上的挂载目录对应在宿主机上的位置。![image-20210914201147338](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210914201147338.png)

### 具名挂载

​	具名挂载就是给互数据卷起了个名字，容器外对应的目录会在/var/lib/docker/volume

```shell
# docker run -di -v 容器挂载点名称:挂载点位置 --name 容器名 镜像
```

```shell
# docker  run -di -v docker_centos_data:/usr/local/niming --name niming  centos:8
```

![image-20210914203916540](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210914203916540.png)

### 只读/读写

### volumes-from（继承）

```shell
// 容器centos7-01指定挂载目录
# docker run -di -v /mydata/docker_centos/data:/usr/local/data --name centos7-01 centos:7
// 容器centos7-04 和centos7-05 相当于继承centos7-01容器的挂载目录
# docker run -di --volumes-from centos7-01 --name centos7-04 centos:7
# docker run -di --volumes-from centos7-01 --name centos7-05 centos:7
```



## 14.查看容器IP

可以通过 dokcer inspect 容器名称|容器ID查看容器的元信息

```shell
# docker inspect niming01
```

![image-20210914204949452](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210914204949452.png)

```shell
# docker inspect --format='{{.NetworkSettings.IPAddress}}' niming01
```

![image-20210914205537140](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210914205537140.png)

