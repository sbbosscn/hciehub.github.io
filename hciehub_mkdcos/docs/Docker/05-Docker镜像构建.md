# Docker镜像构建

​	我们可以通过公关仓库拉取镜像使用，但是，有些时候公共仓库拉取的镜像并不符合我们的需求，尽管已经从繁琐的部署工作中解放出来，但是实际开发时，我们希望镜像包含整个项目的完整环境，在其他机器上拉取打包完整的镜像，直接运行即可。

- docker commit:从容器创建一个新的镜像
- docker build:配合Dockerfile文件创建镜像

1.创建容器

![image-20210915102430231](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210915102430231.png)

2.拷贝资源

![image-20210915102646421](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210915102646421.png)

3.解压资源；调整配置文件

```shell
# cd /root/
# mkdir -p /usr/local/java
# mkdir -p /usr/local/tomcat
# tar -zxvf apache-tomcat-9.0.53.tar.gz -C /usr/local/tomcat
# tar -zxvf openlogic-openjdk-8u262-b10-linux-x64.tar.gz -C /usr/local/java
# vi /etc/profile
```

![image-20210915104219646](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210915104219646.png)

![image-20210915104250795](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210915104250795.png)

4.进入到tomcat目录启动tomcat

```shell
# bin/startup.sh
```

![image-20210915104407079](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210915104407079.png)

5.查看tomcat是否能够成功启动

```shell
# curl http://localhost:8080
```

![image-20210915104623536](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210915104623536.png)

6.删除镜像内的安装包

![image-20210915105459659](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210915105459659.png)

7.构建镜像

![image-20210915105715981](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210915105715981.png)

8.查看构建的镜像

![image-20210915105736552](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210915105736552.png)

9.启动镜像

![image-20210915110001905](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210915110001905.png)

![image-20210915110053414](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210915110053414.png)

![image-20210915110150337](https://gitee.com/hanstack/hanstack_image/raw/master/img/image-20210915110150337.png)



软件路径

[opeJDK8](https://builds.openlogic.com/downloadJDK/openlogic-openjdk/8u262-b10/openlogic-openjdk-8u262-b10-linux-x64.tar.gz)

[tomcat](https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.53/bin/apache-tomcat-9.0.53.tar.gz)