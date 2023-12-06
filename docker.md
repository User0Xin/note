# docker常用命令

##  帮助命令

```shell
docker version # 显示 Docker 版本信息。
docker info # 显示 Docker 系统信息，包括镜像和容器数。。
docker --help # 帮助
```

## 镜像命令

### docker images 列出主机上的所有镜像

``` shell
docker images #列出主机上的所有镜像
[root@kuangshen ~]# docker images
REPOSITORY   TAG   IMAGE ID       CREATED     SIZE
hello-world latest bf756fb1ae65 4 months ago 13.3kB

# 解释
REPOSITORY 镜像的仓库源
TAG 镜像的标签
IMAGE ID 镜像的ID
CREATED 镜像创建时间
SIZE 镜像大小
# 同一个仓库源可以有多个 TAG，代表这个仓库源的不同版本，我们使用REPOSITORY：TAG 定义不同
的镜像，如果你不定义镜像的标签版本，docker将默认使用 lastest 镜像！
# 可选项
-a： 列出本地所有镜像
-q： 只显示镜像id
--digests： 显示镜像的摘要信息
```

### docker search 搜索镜像

```shell
[root@kuangshen ~]# docker search mysql #搜索镜像
NAME DESCRIPTION STARS
OFFICIAL
mysql MySQL is a widely used, open-source relation… 9484
[OK]
# docker search 某个镜像的名称 对应DockerHub仓库中的镜像
# 可选项
--filter=stars=50 ： 列出收藏数不小于指定值的镜像。 
```



### docker pull 下载镜像

```shell
 docker pull mysql #下载镜像
```

 

### docker rmi 删除镜像

```shell
docker rmi -f mysql

# 删除镜像
docker rmi -f 镜像id # 删除单个
docker rmi -f 镜像名:tag 镜像名:tag # 删除多个
docker rmi -f $(docker images -qa) # 删除全部
```



## 容器命令

### docker run 新建容器并启动

```shell
docker run [OPTIONS] IMAGE [COMMAND][ARG...]

# 常用参数说明
--name="Name" # 给容器指定一个名字
-d # 后台方式运行容器，并返回容器的id！
-i # 以交互模式运行容器，通过和 -t 一起使用
-t # 给容器重新分配一个终端，通常和 -i 一起使用
-P # 随机端口映射（大写）
-p # 指定端口映射（小结），一般可以有四种写法
ip:hostPort:containerPort
ip::containerPort
hostPort:containerPort (常用)
containerPort

#例子：在8080端口启动tomcat
docker run -p 8080:8080 tomcat

# 使用centos进行用交互模式启动容器，在容器内执行/bin/bash命令！
[root@kuangshen ~]# docker run -it centos /bin/bash
[root@dc8f24dd06d0 /]# ls # 注意地址，已经切换到容器内部了！
bin etc lib lost+found mnt proc run srv tmp var
dev home lib64 media opt root sbin sys usr


```



### exit 停止退出容器

```shell
[root@dc8f24dd06d0 /]# exit # 使用 exit 退出容器
exit
[root@kuangshen ~]#

exit # 容器停止退出
ctrl+P+Q # 容器不停止退出
```



### docker ps 列出所有运行中的容器

- 使用exit退出的容器会停止运行，ps列出的容器就没有他

- 使用ctrl+P+Q退出的容器会在后台继续运行，ps列出的容器就会有他

```shell
# 命令
docker ps [OPTIONS]
# 常用参数说明
-a # 列出当前所有正在运行的容器 + 历史运行过的容器，会列出已停止但没有删除的容器，可以使用启动的命令再次启动
-l # 显示最近创建的容器
-n=? # 显示最近n个创建的容器
-q # 静默模式，只显示容器编号。
```



### 启动停止容器

```shell
docker start (容器id or 容器名) # 启动容器
docker restart (容器id or 容器名) # 重启容器
docker stop (容器id or 容器名) # 停止容器
docker kill (容器id or 容器名) # 强制停止容器
```



### 删除容器

```shell
docker rm 容器id # 删除指定容器
docker rm -f $(docker ps -a -q) # 删除所有容器
docker ps -a -q|xargs docker rm # 删除所有容器
```



## 其他常用命令

### 后台启动容器

```shell
# 命令
docker run -d 容器名
# 例子
docker run -d centos # 启动centos，使用后台方式启动
# 问题： 使用docker ps 查看，发现容器已经退出了！
# 解释：Docker容器后台运行，就必须有一个前台进程，容器运行的命令如果不是那些一直挂起的命
# 令，就会自动退出。
# 比如，你运行了nginx服务，但是docker前台没有运行应用，这种情况下，容器启动后，会立即自
# 杀，因为他觉得没有程序了，所以最好的情况是，将你的应用使用前台进程的方式运行启动。
```



### 查看日志

```shell
# 命令
docker logs -f -t --tail 容器id
```



### 查看容器中运行的进程信息，支持ps命令参数

```shell
# 命令
docker top 容器id
# 测试
[root@kuangshen ~]# docker top c8530dbbe3b4
UID PID PPID C STIME TTY TIME CMD
root 27437 27421 0 16:43 ? 00:00:00 /bin/sh -c ....
```



### 查看容器/镜像的元数据

```shell
# 命令
docker inspect 容器id
# 测试
[root@kuangshen ~]# docker inspect c8530dbbe3b4
[
    {
        # 完整的id，有意思啊，这里上面的容器id，就是截取的这个id前几位！
        "Id":
            "c8530dbbe3b44a0c873f2566442df6543ed653c1319753e34b400efa05f77cf8",
            "Created": "2020-05-11T08:43:45.096892382Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo kuangshen;sleep 1;done"
        ],
        # 状态
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 27437,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-05-11T08:43:45.324474622Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
    	},
	// ...........
]
```



### 进入正在运行的容器

- docker exec -it 容器id bashShell
- docker attach 容器id

```shell
# 命令1
docker exec -it 容器id bashShell
# 测试1
[root@kuangshen ~]# docker ps
CONTAINER ID IMAGE COMMAND CREATED
STATUS PORTS NAMES
c8530dbbe3b4 centos "/bin/sh -c 'while t…" 12 minutes
ago Up 12 minutes happy_chaum
[root@kuangshen ~]# docker exec -it c8530dbbe3b4 /bin/bash
[root@c8530dbbe3b4 /]# ps -ef
UID PID PPID C STIME TTY TIME CMD
root 1 0 0 08:43 ? 00:00:00 /bin/sh -c while true;do
echo kuangshen;sleep
root 751 0 0 08:56 pts/0 00:00:00 /bin/bash
root 769 1 0 08:56 ? 00:00:00 /usr/bin/coreutils --
coreutils-prog-shebang=s
root 770 751 0 08:56 pts/0 00:00:00 ps -ef
# 命令2
docker attach 容器id
# 测试2
[root@kuangshen ~]# docker exec -it c8530dbbe3b4 /bin/bash
[root@c8530dbbe3b4 /]# ps -ef
UID PID PPID C STIME TTY TIME CMD
root 1 0 0 08:43 ? 00:00:00 /bin/sh -c while true;do
echo kuangshen;sleep
root 856 0 0 08:57 pts/0 00:00:00 /bin/bash
root 874 1 0 08:57 ? 00:00:00 /usr/bin/coreutils --
coreutils-prog-shebang=s
root 875 856 0 08:57 pts/0 00:00:00 ps -ef
# 区别
# exec 是在容器中打开新的终端，并且可以启动新的进程
# attach 直接进入容器启动命令的终端，不会启动新的进程
```



### 从容器内拷贝文件到主机上

```shell
# 命令
docker cp 容器id:容器内路径 目的主机路径
```



# docker镜像

## 原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。rootfs (root file system) ，在bootfs之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。



![image-20230923170237250](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923170237250.png)





![image-20230923170244439](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923170244439.png)

![image-20230923170250507](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923170250507.png)

![image-20230923170300158](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923170300158.png)

![image-20230923170307133](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923170307133.png)



## 镜像Commit

**docker commit** **从容器创建一个新的镜像**

```shell
docker commit 提交容器副本使之成为一个新的镜像！
# 语法
docker commit -m="提交的描述信息" -a="作者" 容器id 要创建的目标镜像名:[标签名]
```



**我们在原有的镜像基础上进行改动，并将其包装成为一个新的镜像发布出去，下次就可以直接使用新的镜像**

```shell
# 1、从Docker Hub 下载tomcat镜像到本地并运行 -it 交互终端 -p 端口映射
docker run -it -p 8080:8080 tomcat
# 注意：坑爹：docker启动官方tomcat镜像的容器，发现404是因为使用了加速器，而加速器里的
tomcat的webapps下没有root等文件！
# 下载tomcat官方镜像，就是这个镜像（阿里云里的tomcat的webapps下没有任何文件）
# 进入tomcat查看cd到webapps下发现全部空的，反而有个webapps.dist里有对应文件，cp -r
到webapps下！
root@aba865b53114:/usr/local/tomcat# cp -r webapps.dist/* webapps
# 2、删除上一步镜像产生的tomcat容器的文档
docker ps # 查看容器id
docker exec -it 容器id /bin/bash
/usr/local/tomcat # ce webapps/
/usr/local/tomcat/webapps # ls -l # 查看是否存在 docs文件夹
/usr/local/tomcat/webapps # curl localhost:8080/docs/ # 可以看到 docs 返回的
内容
/usr/local/tomcat/webapps # rm -rf docs # 删除它
/usr/local/tomcat/webapps # curl localhost:8080/docs/ # 再次访问返回404
# 3、当前运行的tomcat实例就是一个没有docs的容器，我们使用它为模板commit一个没有docs的
tomcat新镜像， tomcat02
docker ps -l # 查看容器的id
# 注意：commit的时候，容器的名字不能有大写，否则报错：invalid reference format
docker commit -a="kuangshen" -m="no tomcat docs" 1e98a2f815b0 tomcat02:1.1
sha256:cdccd4674f93ad34bf73d9db577a20f027a6d03fd1944dc0e628ee4bf17ec748
[root@kuangshen /]# docker images # 查看，我们自己提交的镜像已经OK了！
REPOSITORY TAG IMAGE ID CREATED
SIZE
tomcat02 1.1 cdccd4674f93 About a minute
ago 649MB
redis latest f9b990972689 9 days ago
104MB
tomcat latest 927899a31456 2 weeks ago
647MB
centos latest 470671670cac 3 months ago
237MB
# 4、这个时候，我们的镜像都是可以使用的，大家可以启动原来的tomcat，和我们新的tomcat02来
测试看看！
[root@kuangshen ~]# docker run -it -p 8080:8080 tomcat02:1.1
# 如果你想要保存你当前的状态，可以通过commit，来提交镜像，方便使用，类似于 VM 中的快照！
```



# 容器数据卷

![image-20230923194252612](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923194252612.png)





## 使用数据卷

### 方式一：容器中直接使用命令来添加

```shell
# 命令
docker run -it -v 宿主机绝对路径目录:容器内目录 镜像名

# 测试
[root@myCentOS01 ~]# docker run -it -v /home/ceshi:/home centos /bin/bash
```



### 方式二：通过Docker File 来添加（了解）

```shell
# 1、我们在宿主机 /home 目录下新建一个 docker-test-volume文件夹
[root@kuangshen home]# mkdir docker-test-volume
# 说明：在编写DockerFile文件中使用 VOLUME 指令来给镜像添加一个或多个数据卷
VOLUME["/dataVolumeContainer1","/dataVolumeContainer2","/dataVolumeContainer
3"]
# 出于可移植和分享的考虑，我们之前使用的 -v 主机目录:容器目录 这种方式不能够直接在
DockerFile中实现。
# 由于宿主机目录是依赖于特定宿主机的，并不能够保证在所有宿主机上都存在这样的特定目录.
# 2、编写DockerFile文件
[root@kuangshen docker-test-volume]# pwd
/home/docker-test-volume
[root@kuangshen docker-test-volume]# vim dockerfile1
[root@kuangshen docker-test-volume]# cat dockerfile1
# volume test
FROM centos
VOLUME ["/dataVolumeContainer1","/dataVolumeContainer2"]
CMD echo "-------end------"
CMD /bin/bash
# 3、build后生成镜像，获得一个新镜像 kuangshen/centos
docker build -f /home/docker-test-volume/dockerfile1 -t kuangshen/centos . #注意最后有个.
# 4、启动容器
[root@kuangshen docker-test-volume]# docker run -it 0e97e1891a3d /bin/bash #
启动容器
[root@f5824970eefc /]# ls -l
total 56
lrwxrwxrwx 1 root root 7 May 11 2019 bin -> usr/bin
drwxr-xr-x 2 root root 4096 May 11 11:55 dataVolumeContainer1 # 数据卷目录
drwxr-xr-x 2 root root 4096 May 11 11:55 dataVolumeContainer2 # 数据卷目录
drwxr-xr-x 5 root root 360 May 11 11:55 dev
drwxr-xr-x 1 root root 4096 May 11 11:55 etc
drwxr-xr-x 2 root root 4096 May 11 2019 home
.....
# 问题:通过上述步骤，容器内的卷目录地址就已经知道了，但是对应的主机目录地址在哪里呢？
# 5、我们在数据卷中新建一个文件
[root@f5824970eefc dataVolumeContainer1]# pwd
/dataVolumeContainer1
[root@f5824970eefc dataVolumeContainer1]# touch container.txt
[root@f5824970eefc dataVolumeContainer1]# ls -l
total 0
-rw-r--r-- 1 root root 0 May 11 11:58 container.txt
# 6、查看下这个容器的信息
[root@kuangshen ~]# docker inspect 0e97e1891a3d
# 查看输出的Volumes
"Volumes": {
"/dataVolumeContainer1": {},
"/dataVolumeContainer2": {}
},
# 7、这个卷在主机对应的默认位置
```

![image-20230923200338189](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923200338189.png)





## 匿名和具名挂载

```shell
# 匿名挂载
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx
# 匿名挂载的缺点，就是不好维护，通常使用命令 docker volume维护
docker volume ls
# 具名挂载
-v 卷名:/容器内路径
docker run -d -P --name nginx02 -v nginxconfig:/etc/nginx nginx
# 查看挂载的路径
[root@kuangshen ~]# docker volume inspect nginxconfig
[
{
"CreatedAt": "2020-05-13T17:23:00+08:00",
"Driver": "local",
"Labels": null,
"Mountpoint": "/var/lib/docker/volumes/nginxconfig/_data",
"Name": "nginxconfig",
"Options": null,
"Scope": "local"
}
]
# 怎么判断挂载的是卷名而不是本机目录名？
不是/开始就是卷名，是/开始就是目录名
# 改变文件的读写权限
# ro: readonly
# rw: readwrite
# 指定容器对我们挂载出来的内容的读写权限
docker run -d -P --name nginx02 -v nginxconfig:/etc/nginx:ro nginx
docker run -d -P --name nginx02 -v nginxconfig:/etc/nginx:rw nginx
```



## 数据卷容器

![image-20230923200437577](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923200437577.png)

![image-20230923200442773](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923200442773.png)

![image-20230923200450101](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923200450101.png)

![image-20230923200500186](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923200500186.png)

![image-20230923200508010](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923200508010.png)

![image-20230923200516695](C:\Users\admin\Desktop\学习\笔记\docker.assets\image-20230923200516695.png)

**得出结论：**

**容器之间配置信息的传递，数据卷的生命周期一直持续到没有容器使用它为止。**

**存储在本机的文件则会一直保留！**





