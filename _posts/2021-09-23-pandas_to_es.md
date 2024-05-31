---
title: pandas向es写数据
tags: python develop
author: Anyon
aside:
  toc: true
sidebar:
  nav: layouts
---



这个向es里插入数据的方法是我目前使用的一种方法，后续会根据实际需要进一步完善

#### 1 准备工作

向es里插入数据需要先建立索引，首先使用kibana建立一个索引，格式如下：

```
PUT /pv_sls_set   # 索引名
{
	"settings":{
		"number_of_replicas": 1,
		"number_od_shards": 1
	},
	"mappings": {
		"properties":{
			"TM":{
				"type": "date",
				"format": "yyyy-MM-dd HH:mm:ss"
			}
		}
	}
}
```

这样一个名为pv_sls_set的索引名就建好了，接下来可以使用python第三方包elasticsearch进行数据插入。

es索引中不需要实现声明字段，它会根据插入的数据自动添加字段名。

```
import pandas as pd
from elasticsearch import Elasticsearch

# 建立连接
es = Elasticsearch(["192.168.xx.xx"], http_auth=('username', 'password'), port=19200)

# 首先读取文件
df = pd.reas_csv("total.csv")
# 转化成json
df_as_json = df.to_json(orient="records", lines=True)

bulk_data = []
for json_document in df_as_json.split('\n'):
	bulk_data.append(json.loads(json_document))
	if len(bulk_data) > 1000:
		es.bulk(bulk_data)
		bulk_data = []
es.bulk(bulk_data)
```
