---
title: sqlacodegen反向导出表结构
tags: mysql develop
author: Anyon
aside:
  toc: true
sidebar:
  nav: layouts
---



##### mysql数据库

- 生成flask-sqlalchemy样式的表结构类

```
# 以162上的库为例，连接数据库时密码中含有特殊字符的可能会影响字符串的解析，例如@符号，所以需要先进行转义，@符号转义为%40

flask-sqlacodegen "mysql+pymysql://用户名:密码@ip:端口/ipark" --tables 表名 --outfile "models.py"  --flask

# 解释说明
flask-sqlacodegen后面的字符串表示数据库连接（用双引号括起来）， --tables 后面跟表名，可以是多个（用逗号分隔） --outfile后面跟输出到的文件名（双引号括起来） --flask生成flask样式的表结构
```

- 生成sqlalchemy样式的表结构类

```
# 以162上的库为例

flask-sqlacodegen "mysql+pymysql://用户名:密码@ip:端口/ipark" --tables 表名 --outfile "models.py"
# 或者
sqlacodegen "mysql+pymysql://用户名:密码@ip:端口/ipark" --tables 表名 --outfile "models.py"
```

##### oracle数据库

- 生成flask-sqlalchemy样式的表结构类

```
# 以141上数据库为例，注意表名要小写

flask-sqlacodegen "oracle+cx_oracle://用户名:密码@ip:端口/orcl" --tables 表名 --outfile "models.py" --flask
```

- 生成sqlalchemy样式的表结构类

```
# 以141上数据库为例，注意表名要小写

flask-sqlacodegen "oracle+cx_oracle://用户名:密码@ip:端口/orcl" --tables 表名 --outfile "models.py"
# 或者
sqlacodegen "oracle+cx_oracle://用户名:密码@ip:端口/orcl" --tables 表名 --outfile "models.py"
```

##### 附sqlalchemy的help信息

```
usage: sqlacodegen [-h] [--version] [--schema SCHEMA] [--tables TABLES]
                   [--noviews] [--noindexes] [--noconstraints] [--nojoined]
                   [--noinflect] [--noclasses] [--outfile OUTFILE]
                   [url]

Generates SQLAlchemy model code from an existing database.

positional arguments:
  url                SQLAlchemy url to the database

optional arguments:
  -h, --help         show this help message and exit       # 显示此帮助消息并退出
  --version          print the version number and exit     # 打印版本号并退出
  --schema SCHEMA    load tables from an alternate schema  # 从备用模式加载表
  --tables TABLES    tables to process (comma-separated, default: all)  # 要处理的表（逗号分隔，默认值：全部）
  --noviews          ignore views        # 忽略视图
  --noindexes        ignore indexes      # 忽略索引
  --noconstraints    ignore constraints  # 忽略约束
  --nojoined         don't autodetect joined table inheritance            # 不要自动检测连接表继承
  --noinflect        don't try to convert tables names to singular form   # 不要尝试将表名转换为单数形式
  --noclasses        don't generate classes, only tables                  # 不生成类，只生成表
  --outfile OUTFILE  file to write output to (default: stdout)            # 将输出写入的文件（默认值：stdout）
```









