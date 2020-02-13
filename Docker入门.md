# Docker入门 



## 安装Tomcat以及报404解决方案

记录简单的在Docker 上安装Tomcat

首先我是在云服务器上（Centos系统）安装的Docker，我们需要在https://hub.docker.com/ 上查找Tomcat镜像

```
[root@dragon ~]# docker pull tomcat
```

 拉取完官方的Tomcat的镜像后，我们可以在本地镜像列表里查到 REPOSITORY 为 tomcat 的镜像， 

```
[root@dragon ~]# docker images -a
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
tomcat              latest              aeea3708743f        37 hours ago        529MB
```

 接下来，运行容器 

```
[root@dragon ~]# docker run -d -p 7788:8080 tomcat
e095461be1e55eb8c7c2e28017a9fcee082373584754bb4b8b12cd3f079f0a49
```

说明一下:**-p 7788:8080：**将容器的 8080 端口映射到主机的 7788 端口。

这时候查看docker 正在运行的容器：

![image-20200213185123346](C:\Users\温文星\AppData\Roaming\Typora\typora-user-images\image-20200213185123346.png)

 这时候已经在运行了，接下来，我们用浏览器访问， 

![image-20200213185050695](C:\Users\温文星\AppData\Roaming\Typora\typora-user-images\image-20200213185050695.png)

  这时候，很奇怪哦，404错误？我这里检查完服务器端口8080已经开放了，接下来，我们进入tomcat的目录： 

```
[root@dragon ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                   PORTS                    NAMES
e095461be1e5        tomcat              "catalina.sh run"   8 seconds ago       Up 6 seconds             0.0.0.0:7788->8080/tcp   interesting_varahamihira
c6fff6627cb5        hello-world         "/hello"            7 hours ago         Exited (0) 7 hours ago                            sweet_austin
[root@dragon ~]# docker exec -it e095461be1e5 /bin/bash
root@e095461be1e5:/usr/local/tomcat# ls -l
total 164
-rw-r--r--. 1 root root 19318 Feb  5 22:30 BUILDING.txt
-rw-r--r--. 1 root root  5408 Feb  5 22:30 CONTRIBUTING.md
-rw-r--r--. 1 root root 57011 Feb  5 22:30 LICENSE
-rw-r--r--. 1 root root  1726 Feb  5 22:30 NOTICE
-rw-r--r--. 1 root root  3255 Feb  5 22:30 README.md
-rw-r--r--. 1 root root  7136 Feb  5 22:30 RELEASE-NOTES
-rw-r--r--. 1 root root 16262 Feb  5 22:30 RUNNING.txt
drwxr-xr-x. 2 root root  4096 Feb 11 21:53 bin
drwxr-xr-x. 1 root root  4096 Feb 13 10:46 conf
drwxr-xr-x. 2 root root  4096 Feb 11 21:52 include
drwxr-xr-x. 2 root root  4096 Feb 11 21:52 lib
drwxrwxrwx. 1 root root  4096 Feb 13 10:46 logs
drwxr-xr-x. 3 root root  4096 Feb 11 21:52 native-jni-lib
drwxrwxrwx. 2 root root  4096 Feb 11 21:52 temp
drwxr-xr-x. 2 root root  4096 Feb 11 21:52 webapps
drwxr-xr-x. 7 root root  4096 Feb  5 22:28 webapps.dist
drwxrwxrwx. 2 root root  4096 Feb  5 22:26 work
root@e095461be1e5:/usr/local/tomcat# 
```

 然后查看到里面发现有webapps和webapps.dist两个文件，而wenapps里面没有东西，webapps.dist才是我们要的东西 

```
root@e095461be1e5:/usr/local/tomcat# cd ./webapps
root@e095461be1e5:/usr/local/tomcat/webapps# ls -l
total 0
```

 所以这里把webapps删掉，把webapps.dist改名为webapps 

```
root@e095461be1e5:/usr/local/tomcat# rm -rf webapps
root@e095461be1e5:/usr/local/tomcat# mv webapps.dist webapps
```

 改完之后，我们再重新访问： 

![image-20200213185648501](C:\Users\温文星\AppData\Roaming\Typora\typora-user-images\image-20200213185648501.png)

