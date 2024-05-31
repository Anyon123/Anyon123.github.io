---
title: git常用命令
tags: develop
author: Anyon
aside:
  toc: true
sidebar:
  nav: layouts
---



```
# 建立本地仓库
git clone ssh://--.git

# 切换分支
git checkout 分支名

# 创建远程分支
git checkout -b test  # 1 先在本地创建一个名为test的分支，该分支是基于当前本地所在的分支而建的
git push --set-upstream origin test  # 2 把本地test分支推送到远程

# git提交流程
1 git add .  # 把本地所有修改提交到暂存区
2 git commit -m "本次提交简介"  # 添加本次提交内容说明
3 git pull origin test  # 提交之前先把远程代码和本地代码合并一下，如果有冲突先解决冲突，之后再进行1、2步操作
4 git push origin test  # 将代码提交到远程

# 查看提交记录
git log
# 退出查看
q
```

