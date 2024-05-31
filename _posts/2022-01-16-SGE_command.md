---
title: SGE常用命令
tags: develop
author: Anyon
aside:
  toc: true
sidebar:
  nav: layouts
---



SGE常用指令：

```
# 交互式提交任务
qrsh
例：
qrsh cd 工作目录 && python目录 -m 脚本目录 -n 0

# 批量提交任务。qsub不是交互式的，所以可以批量提交，这里介绍一种批量提交的实现方法。
1 首先建立一个可执行的脚本，例：
run.sh
#!/bin/bash
cd 工作目录
python目录 -m 脚本目录 -n $1

run.sh脚本接收一个参数，放到了-n那里，这是to_es_zgy脚本所需要的参数
然后再写一个可执行脚本，例：
qsub_script.sh
#!/bin/bash
idx_list="0 7 21 28 35 42 49 56 63 70 77 84"
for idx in $idx_list
do
	qsub -N mjob$idx -V -cwd -S /bin/bash run.sh $idx
done

注意两个脚本放在同一个文件夹中，运行./qsub_script.sh即可。

# 查看任务状态
qstat

# 删除任务
qdel -u user  # 删除用户user的任务
qdel -f 81692   # 删除任务id为81692的任务
```

