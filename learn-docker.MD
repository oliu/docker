<!-- TOC -->

* [Docker入门介绍及简单应用](#docker入门介绍及简单应用)
    - [概念](#概念)
    - [安装](#安装)
    - [Docker运行效果演示](#docker运行效果演示)
    - [架构](#架构)
    - [常用命令](#常用命令)
        - [镜像（image）常用命令](#镜像image常用命令)
        - [容器（container）常用命令](#容器container常用命令)
        - [仓库（repository）常用命令](#仓库repository常用命令)
    - [Dockerfile](#dockerfile)
    - [Docker Compose](#docker-compose)
        - [安装](#安装-1)
        - [常用命令](#常用命令-1)

<!-- /TOC -->
# Docker入门介绍及简单应用

## 概念

Docker是一个使用Go语言开发的开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到机器上。

> [官方网站](https://www.docker.com) [官方仓库](hub.docker.com) [官方文档](docs.docker.com)

## 安装

1. 进入[docker官网](https://www.docker.com)
2.  

![image](../images/downdocker.png)

> 备注：a.window系统需要Windows10 Pro，window7版本支持不了 <br/>
> b.macOS系统也可以采用以下命令进行安装

``` 
brew cask install docker
```

## Docker运行效果演示

从仓库中拉取镜像learn-docker，然后输入启动命令：

``` 
docker run -p 80:80 -d --name learn-docker learn-docker
```

## 架构

![image](../images/dockerArch.png)

## 常用命令

``` 
docker COMMAND --help
```

### 镜像（image）常用命令

* docker build -t 镜像名 .--------------------构建镜像 （-t 是打标签)
* docker images -----------------------------镜像列表
* docker rmi 镜像id --------------------------删除镜像

### 容器（container）常用命令

* docker ps --------------------------------查看列表（也可以是docker container ls）
* docker start 容器id/容器name -----------------启动容器
* docker logs 容器id/容器name-------------------获取容器的输出信息
* docker stop 容器id/容器name ------------------停止容器
* docker stop $(docker container ls -q) ----停止所有正在运行的容器
* docker exec -it --------------------------在容器内开启一个交互模式的终端
* docker rm 容器id/容器name --------------------删除容器（+ -f 删除运行中；+ -v一起删除数据卷）
* docker container prune -------------------批量清理已经停止的容器

### 仓库（repository）常用命令

* docker login url -------------------------登录到仓库
* docker tag 原镜像名 镜像名 ----------------------重命名
* docker push 镜像名 --------------------------推送镜像到到仓库
* docker pull 镜像名 --------------------------从仓库里拉取镜像

## Dockerfile

> [官网介绍](https://docs.docker.com/engine/reference/builder/#from)

tips: 指令不区分大小写。但是，命名约定为全部大写。

* FROM 《image name》

tips: FROM命令可以多次使用，表示会创建多个镜像

* MAINTAINER 《author name》
* RUN 《command》

tips: command是在shell或者exec的环境下执行的命令

* ADD 《source》 《destination》

  
tips: source可以是URL或者是启动配置上下文中的一个文件，destination是容器内的路径。

* CMD：提供了容器默认的执行命令。 Dockerfile只允许使用一次CMD指令。 使用多个CMD会抵消之前所有的指令，只有最后一个指令生效。CMD有三种形式:
  + CMD ["executable", "param1", "param2"]
  + CMD ["param1", "param2"]
  + CMD command param1 param2

* EXPOSE 《port》
* ENTRYPOINT：配置给容器一个可执行的命令，这意味着在每次使用镜像创建容器时一个特定的应用程序可以被设置为默认程序。同时也意味着该镜像每次被调用时仅能运行指定的应用。Docker只允许一个ENTRYPOINT，多个ENTRYPOINT会抵消之前所有的指令，只执行最后的ENTRYPOINT指令。语法如下：

   + ENTRYPOINT ["executable", "param1", "param2"]
   + ENTRYPOINT command param1 param2

* WORKDIR：指定RUN、CMD与ENTRYPOINT命令的工作目录。语法如下：

 WORKDIR /path/to/workdir

* ENV：设置环境变量。它们使用键值对，增加运行程序的灵活性。语法如下：

 ENV 《key》 《value》

* USER：镜像正在运行时设置一个UID。语法如下： USER 《uid》
* VOLUME：授权访问从容器内到主机上的目录。语法如下：VOLUME ["/data"]

vue项目Dockerfile示例

``` 
FROM nginx
MAINTAINER oliu <xx@gmail.com>

COPY dist/  /usr/share/nginx/html/
```

java项目Dockerfile示例

``` 
FROM java:8-jre
MAINTAINER oliu <xx@gmail.com>

ADD ./target/xx-web-0.0.1-SNAPSHOT.jar /app/
CMD ["java", "-Xmx200m", "-jar", "/app/xx-web-0.0.1-SNAPSHOT.jar"]

EXPOSE 8081
```

## Docker Compose

Docker Compose项目由Python编写，负责快速的部署分布式应用

### 安装

``` 
sudo pip install -U docker-compose
```

### 常用命令

* docker-compose ps ---------------------------列出项目中目前的所有容器
* docker-compose -f docker-compose.yml config -检查文件的编写正确性
* docker-compose -f docker-compose.yml up -----启动
* docker-compose down -------------------------停止并删除启动已经存在的服务容器
* docker-compose start ------------------------启动
* docker-compose stop -------------------------停止
* docker-compose rm -f ------------------------删除
