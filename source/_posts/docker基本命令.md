---

title: docker基本命令
tags: [Docker]
index_img: https://p.pstatp.com/origin/pgc-image/8fc4d3cf59964cd2b39fa8068e394b59
banner_img: https://p.pstatp.com/origin/pgc-image/8fc4d3cf59964cd2b39fa8068e394b59
categories:
- [技术文档]
date: 2021-09-11 10:00:00

---

# **docker基本命令**

## **基本命令**

### **下载镜像**

```
# 以redis为例子
docker pull redis
```

### **运行镜像**

```
docker run \\
-d \\ # 后台运行
--name redis6 \\ # 自定义名字
-p 6379:6379 \\ # 端口映射
redis # 镜像名称
docker run -d --name redis6 -p 6379:6379 redis redis-server --appendonly yes --requirepass "123456" # 完整命令
```

### **进入容器**

第一种

```
docker exec -it 容器id /bin/bash
```

第二种(不推荐，当退出容器使用exit命令时，会停止这个容器)

```
docker attach 容器id
```

### **暂停容器**

```
docker stop 容器id
```

### **启动容器**

```
docker start 容器id
```

### **查询容器列表**

```
docker ps -a # 查看所有容器
docker ps # 查看运行中的容器
```

**run和start**的区别：

- run是创建一个新的容器
- start是把已经创建好的容器启动

### **查看容器信息**

```
docker inspect 容器id
```

## **挂载**

### **挂载介绍**

容器里面的文件都是在容器内部，而跟你当前电脑是没有关系的，如果删除了容器怎么办？但是资料又想保存就像mysql一样，我只是换一台电脑就要把整个容器复制过去，太麻烦了！所以需要把容器的文件跟当前主机文件作为一个**映射**

### **命令教程**

- 参数-v 宿主机路径:容器路径

```
# 以mysql为例子
docker run -d --name mysql8 -p 3306:3306 -v /data/mysql8/config:/etc/mysql/conf.d -v /data/mysql8/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 mysql
# 以上的命令可以参考https://hub.docker.com/_/mysql里面有详细介绍
```

### **查看容器挂载信息**

```
docker inspect container_name | grep Mounts -A 20 # 显示container_name容器中挂载信息，显示20行
```

### **为什么有知道这么多路径或者参数**

- 每个中间件或者一个数据库容器，他可能需要有很多配置，例如密码，持久化文件的路径等等。那我们怎么知道路径是什么

1. 可以进[hub.docker.com](https://hub.docker.com/) 找到自己需要的容器然后看文档
2. 进容器找

## **网络**

### **容器之间怎么进行通讯**

容器虽然是能相互通讯的，但是容器每次重启ip都跟上一次不一样，所以这样通讯会很复杂

### **示范**

```
# 先拉个centos镜像下来
docker pull centos
# 创建个容器
docker run -d -it --name centos1 centos
docker run -d -it --name centos2 centos

docker inspect centos1_id
```

- 截取一些容器信息下来

```
[
    {
        "NetworkSettings": {
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "9e7ed6d29ca3474de04409833e39b7c7965c7c63d3a1f509886a7a998e4825f8",
                    "EndpointID": "41230bf523fac8fa4933989d98baaaa7655fba5c5dadd14e63839ffe868ed3f8",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.4",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:04",
                    "DriverOpts": null
                }
            }
        }
    }
]
docker inspect centos2_id
[
    {
        "NetworkSettings": {
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "9e7ed6d29ca3474de04409833e39b7c7965c7c63d3a1f509886a7a998e4825f8",
                    "EndpointID": "8ae77d46887c795983ee7a8fb96951d05e236b4ca4b4caa5d5964f892e18a476",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.5",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:05",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

- 以上centos1 ip为**172.17.0.4**
- 以上centos2 ip为**172.17.0.5**

### **解决问题**

```
docker network create centos-network
docker run -d -it --network centos-network --name centos3 centos
docker run -d -it --network centos-network --name centos4 centos
docker exec -it centos3_id /bin/bash
ping centos4
# 所以当创建了一个network后 容器都能加入到这个网络里面，很方便
```

# **docker-compose**

## **Compose 简介**

Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。

Compose 使用的三个步骤：

- 使用 Dockerfile 定义应用程序的环境。
- 使用 docker-compose.yml 定义构成应用程序的服务，这样它们可以在隔离环境中一起运行。
- 最后，执行 `docker-compose up` 命令来启动并运行整个应用程序。

实例：

```
# yaml 配置实例
version: '3' 			# 表示该 Docker-Compose 文件使用的是 Version 3 file
services:
  web:  				# 指定服务名称
    build: .  			# 指定 Dockerfile 所在路径
    ports:				# 指定端口映射
   	  - "5000:5000"
    volumes:			# 挂载
   	  - .:/code
      - logvolume01:/var/log
    links:				# 将指定容器连接到当前连接，可以设置别名，避免ip方式导致的容器重启动态改变的无法连接情况
   	  - redis
  redis:
    image: redis
volumes:				# 卷挂载路径
  logvolume01: {}
```

![https://www.likeyl.cn/2021/06/17/docker基本命令/docker-compose.png](https://www.likeyl.cn/2021/06/17/docker%E5%9F%BA%E6%9C%AC%E5%91%BD%E4%BB%A4/docker-compose.png)

# **Dockerfile**

```
#轻型scratch镜像
FROM golang:alpine AS builder

# 为我们的镜像设置必要的环境变量
ENV GO111MODULE=on \\
    CGO_ENABLED=0 \\
    GOOS=linux \\
    GOARCH=amd64 \\
    GOPROXY=https://goproxy.cn,direct

# 移动到工作目录：/build
WORKDIR /build

# 将代码复制到容器中
COPY . .

# 将我们的代码编译成二进制可执行文件 app
RUN go build -o app .

###################
# 接下来创建一个小镜像
###################
FROM scratch

# 复制文件或文件夹到镜像中
#COPY data/log /log

## 移动到工作目录：/svc
#WORKDIR /cmd

# 从builder镜像中把/build/app 拷贝到当前目录
COPY --from=builder /build/app .
# 声明服务端口
EXPOSE 40005

# 需要运行的命令
ENTRYPOINT ["/app"]
```