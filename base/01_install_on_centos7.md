安装docker-ce
==============
https://yeasy.gitbooks.io/docker_practice/install/centos.html
https://www.jianshu.com/p/d9dbf7e23722

系统要求
--------------
Docker CE 支持 64 位版本 CentOS 7，并且要求内核版本不低于 3.10。 CentOS 7 满足最低内核的要求，但由于内核版本比较低，部分功能（如 overlay2 存储层驱动）无法使用，并且部分功能可能不太稳定。


安装下yum源，这一步根据自已情况来决定是否需要
> rpm -ivh http://xxx/xxx-release-7-1.el7.x86_64.rpm


卸载旧版本
--------------
旧版本的 Docker 称为 docker 或者 docker-engine，使用以下命令卸载旧版本：
```sh
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

安装依赖包：
--------------
```sh
sudo yum install -y yum-utils \
           device-mapper-persistent-data \
           lvm2
```


安装 Docker CE
--------------
鉴于国内网络问题，强烈建议使用国内源。
添加docker-ce的 yum 软件源：
```sh
# 官方源
sudo yum-config-manager \
     --add-repo \
     https://download.docker.com/linux/centos/docker-ce.repo

# 阿里源
sudo yum-config-manager \
     --add-repo \
     https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

更新 yum 软件源缓存，并安装 docker-ce。
> sudo yum makecache fast
> sudo yum install docker-ce


注意执行，上面是安装的最新版本，可以会报出一些依赖包版本过低的错误，比如：container-selinux包版本低。  
两种解决方案：  
	1 哪个包版本低，就升级依赖包  
	2 不装最新版的docker-ce，装一个指定版本的  


查看docker-ce所有可安装的版本
--------------
> yum list docker-ce --showduplicates|sort -r
```sh
Loaded plugins: langpacks
Installed Packages
docker-ce.x86_64            3:19.03.4-3.el7                     docker-ce-stable
docker-ce.x86_64            3:19.03.3-3.el7                     docker-ce-stable
docker-ce.x86_64            3:19.03.2-3.el7                     docker-ce-stable
docker-ce.x86_64            3:19.03.1-3.el7                     docker-ce-stable
docker-ce.x86_64            3:19.03.0-3.el7                     docker-ce-stable
docker-ce.x86_64            18.03.1.ce-1.el7.centos             docker-ce-stable
```


安装指定版本的docker-ce
> sudo yum install docker-ce-18.03.1.ce



启动 Docker CE
--------------
```sh
sudo systemctl enable docker

#执行结果:
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.

#执行sudo systemctl enable docker，会在system目录下创建服务链接
```

```sh
sudo systemctl start docker

#执行结果：

```

其它命令
```sh
重启
sudo systemctl restart docker
```

注意：启动时如果报错。
查看/etc/docker/daemon.json文件，如果为空时，至少应该为'{}'，即空的json对象，不能没有内容



测试 Docker 是否安装正确
--------------
```sh
sudo docker run hello-world

#执行结果
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
d1725b59e92d: Pull complete
Digest: sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
```





如果要单独升级相应依赖包，可去下面地址找   
http://ftp.riken.jp/Linux/cern/centos/7/extras/x86_64/Packages/  
http://mirror.centos.org/centos/7/extras/x86_64/Packages/  
比如找：container-selinux-2.95-2.el7_6.noarch.rpm  



升级DOCKER CE
--------------
要升级Docker CE，需要下载新的软件包文件，并重复安装过程，使用yum -y upgrade 而不是使用yum -y install。


卸载Docker CE
--------------
1）卸载Docker包：
> sudo yum remove docker-ce

2）主机上的镜像，容器，存储卷或自定义的配置文件不会被自动删除。您需要通过执行下面的命令要删除所有镜像，容器和存储卷：
> sudo rm -rf /var/lib/docker

另外，配置文件需要手动删除。






安装docker-compose
================
二进制包  
在 Linux 上的也安装十分简单，从 官方 GitHub Release 处直接下载编译好的二进制文件即可。

例如，在 Linux 64 位系统上直接下载对应的二进制包。  
```sh
sudo curl -L https://github.com/docker/compose/releases/download/1.24.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

查看版本  
> /usr/local/bin/docker-compose version


