# 03-Docker镜像详解

​	Docker中的镜像就像停止运行的容器（类）。实际上你可以停止某个容器的运行并从中创建新的镜像。在该前提下，镜像可以理解为一种构建时（build-time）结构，而容器可以理解为一种运行时（run-time）结构。

![2381630918888_.pic_hd](https://gitee.com/hanstack/hanstack_image/raw/master/img/2381630918888_.pic_hd.jpg)

## 1.镜像和容器

​	通常使用docker container run 和docker container service create命令从某个镜像启动一个或多个容器。一旦容器从镜像启动之后，二者之间的关系就变成了相互依赖的关系。并且在镜像上的容器全部停止之前，镜像是无法被删除的。尝试删除正在被使用的镜像会导致错误。

![image-20210906182203363](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906182203363.png)



如下：

![image-20210906180913110](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906180913110.png)

​	关于镜像和容器可以这么理解。镜像由多个层组成，每层叠加之后，从外部看来就如一个独立的对象。镜像内部是一个精简的操作系统（OS），同时还包含应用运行所必须的文件和依赖包。因为容器是设计初衷就是快速和小巧，所以镜像通常比较小。容器目的就是运行应用和服务，这意味容器的镜像中必须包含应用/服务运行的操作系统和应用文件，但是上面说了，容器是追求快速和小巧。这意味着构建镜像的时候通常需要裁减掉不必要的部分，保持较小的体积。

​	Docke镜像通常只有一个精简的shell，甚至没有shell。镜像中不包含**内核**——容器都是共享Docker所在主机的**内核**。所以有时会说容器仅包含必要的操作系统（通常只有操作系统文件和文件系统对象）。

​	对于一个镜像来说通常它有多层。构成但是构成镜像的每一层都是只读的，当运行docker container run命令之后根据相应的镜像创建容器。创建出的容器实际上就是在镜像的基础上添加了一个可供读写操作的层。这有点类似于VM中的链接克隆技术。

## 2.拉取镜像

Linux Docker主机本地镜像仓库通常位于/var/lib/docker/[storage-dirver],在此处的storage-dirver指的是使用的存储驱动的类型。在不同版本的docker中使用的存储驱动类型并不总是一致的。本次安装的Docker 版本为：Docker Version：20.10.8 Storage Driver：overlay。除此之外Storage Diver还有extfs，overlay2等。

​	例子：

使用：docker system info可以查看docker的信息,其中包含存储驱动类型。

```shell
# docker system info
```

![image-20210906215032969](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906215032969.png)	

```shell
# cd /var/lib/docker/overlay
# ll
```

![image-20210906214450299](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906214450299.png)

上图中蓝色字符为镜像的Image ID

```shell
# docker image ls
```

![image-20210906214838822](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906214838822.png)

值得说明的一点是，在使用docker image ls命令时。显示出来的Image ID实际上并不是完整的Image ID是实际Image ID的最后几位。

拉取镜像：

```
# docker image pull node:latest
```

![image-20210906221431664](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906221431664.png)

## 3.镜像来源Docker Hub

Docker镜像存储在镜像仓库服务（Image Registry）当中。Docker客户端的镜像仓库服务是可以配置的，默认使用的是Docker Hub。

​	镜像仓库服务包含多个镜像仓库（Image Registry）。同样，一个镜像仓库中也会包含多个镜像。

## 4.镜像的命名和标签

## 5.拉取镜像

​	只要给出镜像的名字和标签就能够从官方的镜像仓库中定位一个镜像。镜像的名字和标签之间使用的的是“:”进行分割，例如从官方拉取镜像时docker image pull的命令格式如下。

​	docker image pull [repostiory:latest]

从mongo仓库中拉取标签为3.2.0-rc1的镜像

```shell
# docker image pull mongo:3.2.0-rc1
```

**注意：**

​	如果没有在镜像仓库名称后面指定具体的镜像标签，则Docker会默认拉取标签为latest的镜像。但是latest标签是一个非强制标签，也就是说带有latest标签的镜像并不能够保证该镜像是仓库中最新的镜像。

## 6.过滤docker image ls的输出

docker使用 --filter参数来实现对docker image ls输出的过滤。	

​	下面的实例会返回本地镜像中的悬虚镜像。

```shell
# docker image ls --filter dangling=true
```

![image-20210906230048181](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906230048181.png)

悬虚镜像是那些没有标签的镜像。在列表中展示为**[none]:[none]**。通常出现这种情况，是因为构建了一个新的镜像，然后为该镜像打了一个已经存在的标签。

docker支持的过滤器：

- dangling：可以指定true或者flase，返回的结果是悬虚镜像（true）或者非悬虚镜像（false）。

- before：需要镜像名称或者ID作为参数，返回之前被创建的所有镜像

  ```shell
  # docker image ls  --filter brefore=7d0d8fa37224
  ```

  

  ![image-20210907090209390](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907090209390.png)

- since：与before类似，不过返回的是指定镜像之后创建的所有镜像。

  ```shell
  # docker image ls  --filter since=7d0d8fa37224
  ```

  

  ![image-20210907090247745](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907090247745.png)

- label：根据标注（label）的名称或者值对镜像进行过滤

- 使用reference完成过滤并且仅显示标签为latest的镜像

  ```shell
  # docker image ls --filter=reference="*:latest"
  ```

  

  ![image-20210907090735746](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907090735746.png)

  

- 使用--formt参数来通过Go模板对输出的内容进行格式化。

  - 返回主机上镜像大小

```shell
# docker image ls  --format "{{.Size}}"
```



![image-20210907091612889](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907091612889.png)

- 使用下面的命令可以返回全部镜像但是只显示仓库，标签和大小信息。

```shell
#docker image ls --format "{{.Repository}}:    {{.Tag}}:    {{.Size}}"
```

![image-20210907092326218](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907092326218.png)

## 7.镜像检索

docker search命令允许通过CLI的方式搜索Docker Hub。通过NAME进行匹配，并基于返回的内容进行过滤。

```shell
# docker search ubuntu
```

![image-20210907093719372](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907093719372.png)

实际上Docker search默认会返回25条结果；但是我在上面的截图中省略了一部分。



通过Docker内置的help工具可以知道，docker search 可以通过Options参数来过过滤输出的结果。

![image-20210907093643826](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907093643826.png)



值得注意的是，在使用Docker search ubuntu这条命令时默认输出了25条ubuntu仓库的信息。但是这些仓库并不都是官方仓库。如果你想输出的结果只显示官方仓库那么你就可以使用filter参数对输出的结果尽心过滤。

```shell
# docker search ubuntu --filter "is-official=true"
```

![image-20210907094446456](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907094446456.png)

前面说到Docker search默认只会输出25条结果，你可以通过使用limit参数来增加返回的条目数，但是最大不能超过100条。

```shell
# docker search ubuntu --limit 100
```

![image-20210907094800059](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907094800059.png)

![image-20210907094828544](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907094828544.png)

## 8.镜像和分层

​	在前面其实已经提到过镜像是分层的。镜像是由一些松耦合的只读镜像层组成。

![image-20210906182203363](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906182203363.png)

​	docker复制堆叠这些镜像层，并且把他们表示为单一统一的对象。

​	尝试拉取一个镜像。拉取ruby这个镜像，以Already exists结尾的每一行表示本地已经存在的镜像层。以pull complete结尾的是在网上拉取的层。ruby这个镜像总共包含8个层。每个层都使用层ID的形式进行区别。

```shell
# docker image pull ruby:latest
```

![image-20210907095553871](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907095553871.png)

查看镜像分层的另一种方式

```shell
# docker image inspect ruby:latest
```

截取之后的输出结果同样包含8个层，并且层ID是完整的。没层都是使用sha256的三散列值来标识镜像层。

![image-20210907100157937](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907100157937.png)

​	所有的Docker镜像都起始于一个**基础镜像层**，当进行修改或者添加新的内容时，就会在当前镜像层上创建新的镜像层。

base镜像由两层含义：

- 不依赖其他镜像，从scratch构建
- 其他镜像可以基于基础镜像进行扩展。

所以能成为基础镜像的通常为Linux的各种发型版本的Docker镜像，比如ubuntu，Debian，Centos等。base镜像提供的是最小安装的Linux发行版本。

​	最新的ubuntu镜像只有70多M。之所以这么小是因为docker的镜像在运行的时候直接使用docker宿主机的的kernel。

![image-20210907102617143](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907102617143.png)

​	linux OS由kernel（内核）和rootfs（用户空间）组成。不同的linux发行版本的区别在于rootfs而不是kernel。比如ubuntu采用apt来管理软件包而centos采用的是yum。

**注意：**

​	base镜像只是用户空间和发行版一直，kernel使用的是宿主机的kernel。例如我的宿主机为centos7，kernel版本为：3.10.0-862.el7.x86_64。而在宿主机上安装的最新版本的ubuntu镜像的kernel版本也是3.10.0-862.el7.x86_64。

![image-20210907110247897](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907110247897.png)

​	一个简单的例子，基于ubuntu Linux 20.01创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加了python包，就会在基础镜像上创建第二个镜像层；如果继续添加安全补丁，就会创建第三个镜像层，那么该镜像就会包含三个镜像层。如果这个镜像在网络上的仓库上你就可以在使用docker image pull [image:tag] 命令时查看镜像的层数，当然也可以使用docker image  inspect [image:tag]直接查看该镜像的层级。

​	在添加额外镜像层的同时，镜像时钟保持是当前所有镜像层的组合。Docker采用存储引擎（新版本采用快照机制）的方式来实现镜像层的堆栈，并保证多镜像层对外表示为统一文件系统（UnionFS）。统一文件系统是一种分层，轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟机文件系统下(unite several directorire into a single  virtual filesystem)，Union文件系统是Docker镜像的基础。

​	docker镜像采用分层机制实现了资源的共享。在前面拉取ruby:latest镜像的时候。本地已有的镜像层会显示Already exists。由此可知，Docker能够自动识别要拉取的镜像中，哪几层已经在本地存在，已存在的镜像层是可以共享的。

​	Docker在linux上支持了多种存储引擎（Snapshotter）每个存储引擎都有自己的镜像分层、镜像共享和写时复制（COW）技术的具体实现。但是最终实现的效果和体现趋向一致。windows平台仅仅支持一种基于NTFS文件系统的存储引擎。

![image-20210907131856716](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907131856716.png)

​	关于镜像的Cow特性会在容器部分详细说明。

## 9.镜像散列值

​	Docker中的镜像是一些列松耦合的独立层的集合。

​	镜像本事实际上是一个配置对象，其中包含了镜像层的列表和一些列的元数据信息。

​	镜像层才是实际存储数据的地方，每个镜像层之间是完全相互独立的，所以不存在镜像集合的概念。

​	镜像的唯一标识是一个加密的ID，该ID是配置对象本身的散列值，每个镜像层也有一个加密的ID去分，其值是镜像层本身内容的散列值。之前学过一个命令 docker image  inspest [image:latest]。通过这条命令可以查看ImageID和组成Image的各个分层的ID。

以ruby:latest镜像为例

```shell
# docker image inspect ruby:latest
```

Image ID：

![image-20210907133758053](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907133758053.png)

分层ID：

![image-20210907133820219](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907133820219.png)

如果要修改镜像的内容或者是其中的任意一层都会导致加密散列值的变化。所以镜像是不可变更的，每一层镜像发生变更都会被判别。

## 10.删除镜像

​	当镜像不被需要的时候，就可以删除镜像以回收镜像所占用的空间。此时可以使用docker image rm [ImageID]来删除被ImageID标识的镜像。

​	删除操作会在当前主机上删除该镜像的以及相关的镜像层。执行完docker image rm [ImageID]之后不能够时候docker image ls看到被删除的镜像。并且被删除的镜像的所包含的镜像层数据的目录也会被删除，但是如果被删除的镜像当中包含被共享的镜像层。之后当共享该镜像层所有的镜像被删除的才会被删除该共享层。

​	如果被删除的镜像上存在运行状态的容器该镜像也无法被删除会报错。如果要删除该镜像需要在删除镜像之前停止所有使用该镜像的容器。

显示所有镜像的ID

```shell
# docker image ls -q
```

![image-20210907135226157](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907135226157.png)

批量删除镜像：

```shell
# docker image rm $(docker image ls -q) -f
```

![image-20210907140037614](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210907140037614.png)

批量停止容器

```shell
# docker container stop $(docker container ls -q)
```

在批量删除镜像之前可以采用批量停止容器之后再做批量删除操作。

```shell
# docker container stop $(docker container ls -q)
# docker image rm $(docker image ls -q) -f
```

## 11.镜像的导入和导出

### 镜像的导出

![image-20210910214837239](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210910214837239.png)

```shell
# docker save -o /test/myalpine.image alpine:latest
```

或

```shell
# docker save > /test/myalpine.image alpine:latest
```

![image-20210910220241920](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210910220241920.png)

### 镜像的导入

![image-20210910221429556](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210910221429556.png)

```shell
# docker load -i /test/myalpine.image
```

或

```shell
# docker load < /test/myalpine.image
```

