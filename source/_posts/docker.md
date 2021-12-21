---
title: docker
date: 2021-11-26 17:01:27
tags:
---

### docker 命令

+ `docker run`

运行一个`docker`实例

```bash
docker run hello-world
```

+ `docker ps`

列出所有正在运行的`docker`实例

```bash
docker ps
```

+ `docker kill`

关掉一个正在运行的`docker`容器

```bash
docker kill 4607082
```

+ `docker rm hello-world`

删除一个`docker`镜像

```bash
docker rm hello-world
```

### docker  安装 mysql

```bash
docker run --name -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7.27
```

+ `--name` 后面接 `mysql`名字（自定义）

+ `-e` 为设置环境变量

+ `MYSQL_ROOT_PASSWORD` 后面接 `ROOT` 用户的密码

+ `-d` 为开启守护进程

+ `mysql:` 后面接`mysql` 版本

### docker 安装 postgres

```bash
docker run --name some-postgres -p 5432:5432 -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```

+ 参数参考上面`mysql`
+ `-p` 后跟 主机端口`:`容器端口
