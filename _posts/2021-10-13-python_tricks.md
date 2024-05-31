---
title: python小技巧
tags: python develop
author: Anyon
aside:
  toc: true
---



##### 生成项目依赖

```
# 需要安装pipreqs模
pip install pipreqs

# 进入到当前项目根目录，运行即可
pipreqs ./ --encoding=utf8
```

#### json.dump的用法

```
x = [1,2,3,4]
json.dump(x, open("file.txt", "w"))

# 加载时只需
json.load(open("file.txt"))
```

