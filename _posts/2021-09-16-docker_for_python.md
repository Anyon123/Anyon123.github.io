---
title: docker部署python项目
tags: docker python develop
author: Anyon
aside:
  toc: true
sidebar:
  nav: layouts
---



##### 【1】从镜像文件中加载镜像

使用```docker load```命令

```
# docker load 命令用于加载由docker save命令导出的镜像， -i命令后面指定加载的镜像文件名称
docker load -i optimize161.tar
```

##### 【2】生成容器

```
docker run -itd --network host -e TZ=Asia/Shanghai --name optimize optimize:v2 /bin/bash

-i 交互模式运行容器
-t 为容器分配一个伪终端
-d 后台运行，并返回容器id
--network 指定容器网络模式，host为与主机共享网络
--name 指定容器名
/bin/bash 载入容器后运行bash，因为docker必须要保持至少一个进程运行
```

##### 【3】进入容器

```shell
docker exec -it optimize /bin/bash
```

##### 【4】在容器中后台运行程序

```
# 找到程序所在的文件夹，使用nohup启动守护进程

nohup python optimize-server.py &
```

##### 【5】退出容器

```
exit
```





##### 【拓展】从服务器上向容器中拷贝文件

```
docker cp 文件夹/文件名称 容器名:容器中文件夹/文件名称

例如：此命令会将当前服务器中optimize-server文件夹拷贝至容器optimize中的optimize-server20210902文件夹
docker cp optimize-server optimize:optimize-server20210902
```

##### 【拓展】从镜像中导出镜像文件

使用```docker save```命令

```
# -o 表示输出到文件，后面跟.tar结尾的文件名
docker save -o optimize161.tar optimize:v20210518
```

##### 【拓展】从容器中导出docker镜像

```shell
docker export [options] container (-o：写入到文件)
或者
docker export container > 镜像文件名.tar

# 例
docker export -0 optimize-server.tar a404c6c174a2
或
docker export a404c6c174a2 > optimize-server.tar

```

##### 【拓展】从镜像文件中加载镜像

使用```docker import```命令

```shell
docker import 用于加载由docker export命令导出的镜像
docker import [options] 镜像文件名.tar [镜像名[:tag]]

# 例
docker import optimize-server.tar optimize:v2
```

##### 【拓展】docker修改容器中的时间

```
一种方法是在生成容器时指定时区，参考生成容器指令

另一种方法是在生成容器时加上--cap-add SYS_TIME参数，如
docker run -it --cap-add SYS_TIME --name optimize optimize:v2 /bin/bash
进入容器后，使用以下命令修改时间
date -s 08/24/2020
date -s 13:02:00

修改系统时间需要有SYS_TIME权限，--cap-add或--cap-drop可用添加或禁用特定权限，--privileged是将所有权限都给容器开放。
```

##### 【拓展】删除镜像

```
docker rmi 镜像id
```

##### 【拓展】删除容器

```
docker rm 容器id
```

##### 【拓展】运行处于停止状态的容器

```
docker start 容器id  # optimize是容器名称
```

##### 【拓展】停止容器

```
docker stop 容器id  # 优雅地停止
docker kill 容器id  # 直接关闭
```





