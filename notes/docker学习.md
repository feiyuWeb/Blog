### docker 的基本组成

-   Docker Client 客户端
-   Docker Daemon 守护进程
-   Docker Image 镜像
-   Docker Container 容器
-   Docker Registry 仓库

### docker 容器的能力

-   文件系统隔离：每个容器都有自己的 root 文件系统
-   进程隔离：每个容器都运行在自己的进程环境中
-   网络隔离：容器间的虚拟机网络接口和 IP 地址都是分开的
-   资源隔离和分组：使用 cgroups 将 CPU 和内存之类的资源独立分配给每个 Docker 容器

### docker 常用命令

```go
docker images                       # 查看现有镜像
docker image ls                     # 查看现有镜像，和上面等同
# docker image ls -f dangling=true  # 查看虚悬镜像，-f 即 --filter，过滤镜像
docker image pull [imageName]       # 拉取镜像
docker image rm [imageName]         # 删除镜像，等同 docker rmi
docker image prune                  # 删除虚悬镜像

docker container ls [-a]            # 列出正在运行的容器，等同于 docker ps，-a 可查看所有容器
docker container run [hello-world]  # 运行容器
docker container exec [containerID] # 进入容器内部
docker container start              # 启动已经生成、已经停止运行的容器文件
docker container stop [containerID] # 终止容器运行
docker container kill [containID]   # 手动杀死终止容器运行
docker container rm [containerID]   # 删除容器，等同 docker rm
docker container logs [containerID] # 查看 docker 容器的输出
docker container cp [containID]:[/path/to/file] . # 从正在运行的 Docker 容器里面，将文件拷贝到本机
docker container prune              # 删除所有停止运行的容器

docker ps [-a]                      # 查看所有正在运行的容器，-a 可查看所有容器
docker stop [containerID]           # 终止容器运行
docker system df                    # 查看镜像、容器、数据卷所占用的空间

docker volume ls                    # 查看数据卷
docker volume prune                 # 删除无主数据卷
```

### 运行镜像文件

docker run [OPTIONS] IMAGE [COMMAND][arg...]

```go
docker run -p 8081:80 --name mynginx  -d nginx
```

### Dockerfile 指令

-   FROM <可用的镜像名>
-   MAINTAINER <name>
-   RUN
-   EXPOSE

-   CMD // 可以当做 RUN 指令的默认指令，RUN 的优先权更高
-   ENTERYPOINT
-   ADD
-   COPY
-   VOLUME

-   WORKDIR
-   ENV
-   USER

> 例如：
> FROM ubuntu:14.04
> MAINTAINER dormancypress "dormancypress@outlook.com"
> RUN yum update
> RUN yum install -y nginx
> EXPOSE 80

### 编写 Dockerfile 文件

Dockerfile 文件是一个文本文件，用于配置 image，生成自己的 image 镜像。在配置 Dockerfile 文件之前，需要先添加一个文本文件 .dockerignore，用于排除不需要打包进入 image 镜像的文件路径

```go
.git
node_modules
npm-debug.log
```

之后创建 Dockerfile 文本文件，配置如下

```go
FROM node:8.4         // 该 image 镜像继承官方 node 8.4 版本的 image
MAINTAINER chanshiyu  // 标明作者
COPY . /app           // 将除 .dockerignore 排除文件外的所有文件 copy 到 /app 目录
WORKDIR /app          // 指定接下来的工作目录为 /app
RUN npm install       // 安装依赖，安装后的所有依赖都将打包到 image 文件
EXPOSE 3000           // 暴露 3000 端口，允许外部连接这个端口
```

![Dockerfile指令](https://i.loli.net/2019/10/15/A97FQVGICh4B1Lb.png)

### 创建 image 镜像

配置好 Dockerfile 文件之后，即可创建自己的 image 镜像文件

```go
# docker image build -t [username]/[repository]:[tag] .
docker image build -t koa-demo:0.0.1 . // 最后的一个点是表示路劲为当前的路径
```

### Dockerfile 指令

```go
// 指定基础镜像，本地没有会从dockerHub pull下来
FROM java:8
// 作者
MAINTAINER coco
// 把可执行jar包复制到基础镜像的根目录下
ADD luban.jar /luban.jar
// 镜像要暴露的端口，如要使用端口，在执行docker run命令时使用-p生效
EXPOSE 80
// 在镜像运行为容器后执行的命令
ENTRYPOINT ["java","-jar","/luban.jar]
```

```go
// 给镜像指定各种元数据
LABEL <key>=<value>
// 用于从宿主机复制文件到创建的心镜像文件
COPY <src> <dest>
// 用来指定docker build过程中运行指定的命令
RUN <command>
```

> 使用`docker build`命令构建镜像，基本语法

```go
docker build -t coco/mypro:v1
```

-   -f 指定 Dockerfile 文件的路劲
-   -t 指定镜像名字和 TAG
-   . 指定当前目录，这里实际上需要一个上下文路径
