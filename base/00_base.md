



名词解释
===========
镜像：从镜像市场中下载的即为镜像，可以理解为容器的模板。

容器：应用程序运行的环境，容器的创建依赖于某一镜像。注意，容器不是镜像的拷贝，容器只是在镜像之上建立了一层读写层，用以覆盖容器内对镜像配置、文件的修改。采用这一方式可以避免因频繁的镜像复制而导致的资源浪费。具体可以参考相关书籍或博客。



查看 Docker 版本
> docker --version



镜像操作
============
搜索镜像
------------
可以在安装了 Docker 的机器上使用以下指令搜索镜像，不过还是建议通过访问镜像商店的方式搜索。注意，$mirror-name 需要替换为想要搜索的镜像名。
> docker search $mirror-name


获取镜像
------------
可以使用以下指令拉取镜像到本地，其中冒号后的 $tag 为镜像的版本标签，如果省略冒号及之后的内容，则为下载最新版本即 :latest。版本标签信息可以在镜像市场中查找到。
> docker pull $mirror-name:$tag

注意，若下载的镜像携带有版本标签，则之后对这一镜像的使用都需要携带版本标签，否则会因为版本不同而再次下载。


查看镜像
------------
查看所有镜像可以使用：
> docker images

也可通过以下方式查看单个镜像：
> docker images $mirror-name


删除镜像
------------
我们可以通过以下方式删除镜像，但此时需要保证没有容器使用这一镜像：
```sh
docker rmi $mirror-name

#或者
docker image rm $mirror-name
```



容器操作
============
查看已启动的容器
------------
> docker ps

查看全部容器
------------
> docker ps -a

查看容器日志
------------
采用以下方法可以查看容器的操作历史和输出：
> docker logs $container-name



生成容器
------------
可以通过以下方式生成一个基于某一镜像的容器，注意，如果宿主机中没有该镜像则会先进行下载。务必注意镜像标签是否正确。
> docker run $mirror-name:$tag

使用这一命令会使得容器在创建后自动启动。


给容器添加自定义名字
------------
通过 docker ps -a 可以看到容器的 ID 和 name，这两者都可以作为后续对容器删除、启动、关闭及设置等操作的标识。使用 ID 时，只需输入 ID 的前几位即可（能与其他容器区分）。

Docker 会为其随机生成 64 位长度的字符串作为 ID，当然，我们也可以通过如下方式手动指定容器的名字，其中 $container-name 即为指定的容器名。
> docker run --name $container-name $mirror-name


启动容器
------------
启动的是已存在的容器，但是该容器当前是关闭的
> docker start $container-name


关闭容器
------------
> docker stop $container-name

关闭后的容器docker ps看不到，docker ps -a 可以查看到


以交互方式创建容器
------------
可以通过以下方式，以交互的方式创建容器，当然也可以在 $mirror-name 的前面加上 --name xxx 来指定容器的名字，在交互模式中，可以输入 exit 退出：
> docker run -it $mirror-name

以后台运行方式创建容器
------------
我们可以使用 -d 操作使容器在后台运行：
> docker run -d $mirror-name


容器状态问题
------------
容器在启动后，如果没有活动的前台进程，容器会自动关闭。若要保持容器启动状态，可以强制其执行一个前台进程。具体可查看 参考链接 I 和 参考链接 II。

可以用以下方式创建一个不自动关闭的 centos 镜像：
```sh
docker run -it --name mycentos centos
docker start mycentos
```

// 此时可以看到该容器没有自动关闭
docker ps
容器执行操作
我们可以通过以下方式对已经启动的容器执行一些操作，其中 $container-name 可以是容器的名字，也可以是容器的 ID：
> docker exec $container-name echo "hello" && echo "world"

也可以通过以下方式进入交互模式：
> docekr exec -it $container-name bash

其中，&& 是起到操作间连接的作用，详细可以查看 参考链接。此外，我们也可以在创建的容器的时候就使其执行一些操作：
> docker run $mirror-name echo "hello world"

查看容器详情
------------
通过以下方式可以查看容器的详细信息，这些信息是采用 JSON 的格式展现的：
> docker inspect $container-name


删除容器
------------
可以在 rm 之后加入一个或多个容器名或容器 ID 进行批量删除。
> docker rm $container-name-1 $container-name-2 ...

可以使用以下方法删除全部容器：
docker rm $(docker ps -aq)

xx
------------







