# docker 升级教程

## 1.docker升级的准备工作

1.停止Docker的守护进程

2.移除旧版本的Docker

3.安装新版本的Docker

4.配置新版本的Docker为开机自动启动

5.确保容器重启成功

以Centos7为例

将Centos7上的Docker从17.03.0升级到最新版本。

1.查看当前的Docker版本

```shell
# docker version
```

![image-20210906104425733](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906104425733.png)

2.移除旧版本的Docker

```shell
# yum remove docker docker-engine docker-ce docker.io -y
```

![image-20210906104629484](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906104629484.png)

3.升级centos的软件包

```shell
# yum update	
```



3.使用脚本安装最新版本的Docker

```shell
# wget -qO- https://get.docker.com/ | sh
```

4.等待升级完成查看当前的docker版本

```shell
# docker version
```

5.将docker设置为开机自动启动

```shell
# systemctl start docker
# systemctl enable docker
```

6.查看当前的docker版本

```shell
# docker version
```

![image-20210906105441367](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210906105441367.png)

