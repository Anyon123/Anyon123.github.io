---
layout: article
title: linux常用命令
tags: develop
author: Anyon
aside:
  toc: true
sidebar:
  nav: layouts
---



#### 查询ip地址

```
ifconfig
```

#### 关机

```
# 立即关机
shutdown -h now

```

#### 修改ip

```
# 后面那个ifcfg-ensXXX根据实际的网卡名称而定
vi /etc/sysconfig/network-scripts/ifcfg-ensXXX

打开之后主要关注这几个配置
BOOTPROTO=static/dhcp  # 静态ip或动态ip
ONBOOT=yes/no  # 随系统启动而启动
IPADDR=192.168.xxx.xxx  # 自定ip
NETMASK=255.255.255.0  # 子网掩码
GATEWAY=192.168.0.1  # 网关
DNS1=8.8.8.8  # DNS
```

#### 重启网络

```
# centos
service network restart
```

#### 删除

```
# 删除文件夹
rmdir xxx
# 删除文件
rm xxx
```

#### 查找

```
# 查找某个文件或文件夹
# find指查找命令
# /home 查找home目录下的文件
# -name "Doc*" 查找名称为Doc*,*代指0个或多个其他字符
# -type d 查找文件夹，不加则文件、文件夹都查找
# > fff 查找结果输入到fff文件中
find /home -name "Doc*" -type d > fff
```

#### 查找进程端口号

```
lsof -i | grep pid
netstat -nap | grep pid
```

#### 查看文件和文件夹大小

```
# 查看系统中文件使用情况/查看磁盘大小
df -h

# 查看当前目录下各个文件及目录占用空间大小
du -sh *

# 查看当前目录，不输出子目录
du -sh

# 查看分区的文件系统
df -T

```

#### 查找文件夹下所有文件名输出到指定文件

```
find . -name '*' -> 指定文件
```

#### 查看进程资源占用

```
top
```

#### linux打包解压

```
打包
tar -czvf save_data.tar.gz save_data/

解压
tar -zxvf save_data.tar.gz
```

#### 传输文件

```
# 向对方服务器发送文件
scp 本地文件路径 对方用户名@ip:/文件夹
例：
scp save_data.tar.gz user@host:/home/user/

# 从对方服务器复制文件
scp 对方用户名@ip:/文件 /本地文件夹
例：
scp root@192.168.0.1:/home/user/save_data.tar.gz .
```

#### 批量修改文件名

```
# 将当前文件夹下文件名称中含有xxxxxx的部分替换成xxx
rename "xxxxxx" "xxx" *
```

#### 批量删除小文件及小文件夹

```
# 新建一个空文件夹emtpy/
# 要删除的目标文件夹save_inverter/

rsync --delete-before -a -H stats empty/ save_inverter/

# 能不生成小文件就尽量不生成小文件，删除的时候都是泪，rm -rf得删好几天，用rsync也得删好久。最后发现一共生成了700多万个小文件，这肯定导入的慢啊。。。
```

