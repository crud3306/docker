

docker基本概念
-----------
```
Docker Image

Docker Regsitry

Container

Docker Daemon

Docker Client
```


基本命令
------------
```
查看版本
docker version

查看帮助
docker help

去远程registry查找是否存在某镜像image
docker search xxx

列举当前的Docker image
docker image ls   

拉取/更新某image
docker pull [name]:[tag] 

删除某image
docker image rm [xxx]


启动一个容器，如果本地不存在该image，则docker会先自动 docker pull hello-world
docker run hello-world
docker run -p 8000:8080 -d tomcat:8.0
docker run -v ~:/user -it ubuntu bash

#-p port1:port2 #端口映射
	#Port1: host机的端口
	#Port2: Docker实例的端口

#-v dir1:dir2 #目录映射
	#dir1:  host机的目录
	#dir2:  Docker实例的目录

#-it 表示起一个交互终端，来run后面的命令

#-d 表示以守护进程的方式执行

#exit 退出当前shell


查看正在运行的容器
docker ps

查看所有容器，包含运行的和未运行的
docker ps -a

杀掉进程
docker kill [xxx] 

看log
docker logs [container_id]

Inspect
docker inspect [container_id]

在Docker实例中执行shell命令
docker exec -it [container_id] bash

```


Dockerfile使用
============
Dockerfile文件格式，文件名即为Dockerfile
------------
```
用于生成Docker的image的配置文本文件
常用语法：

FROM   基于已有的Docker image来生成 
例如：FROM tomcat:8.0

COPY     把用户的文件copy到image去
例如：COPY index.jsp  /usr/local/tomcat/webapps/ROOT

EXPOSE 对外通过该端口提供服务
例如：EXPOSE 8080

CMD  启动该image应该跑的脚本
例如：CMD ["catalina.sh", "run"]
```


使用Dockerfile文件来启动容器
------------
docker build –t [registry]/[project-id]/[image-name]:[version] .
注意：要使用registry对应的域名，例如百度云使用hub.baidubce.com

docker push [registry]/[project-id]/[image-name]:[version]
把该image 上传到该registry中去


示例
```
Build a Docker image
#-----------
cd node/docker
docker build -t hub.baidubce.com/bootcamp/mynode:1.0.0 .

Push it to CCE Registry
#-----------
docker login --username=guest hub.baidubce.com (pw：xxxxxxx)
docker push hub.baidubce.com/bootcamp/mynode:1.0.0

Pull/Run a Docker image from CCE hub
#-----------
docker run -d -p 8000:8080 hub.baidubce.com/bootcamp/mynode:1.0.0
docker ps |grep mynode

curl localhost:8000
curl 127.0.0.1:8000
```



启动容器
-------------
容器是在镜像的基础上来运行的，一旦容器启动了，我们就可以登录到容器中，安装自己所需的软件或应用程序。既然镜像已经下载到本地，那么如何才能启动容器呢？

只需使用以下命令即可启动容器：

docker run -i -t -v /software/:/mnt/software/ a884aafc2206 /bin/bash
这条命令比较长，我们稍微分解一下，其实包含以下三个部分：
docker run <相关参数> <镜像 ID> <初始命令>

其中，相关参数包括：

-i：表示以“交互模式”运行容器
-t：表示容器启动后会进入其命令行
-v：表示需要将本地哪个目录挂载到容器中，格式：-v <宿主机目录>:<容器目录>
-p:表示需要将本地哪个端口映射到容器中哪个端口，格式：-p <宿主机端口>:<容器端口>



查看正在运行的容器
docker ps

查看所有容器
docker ps -a




