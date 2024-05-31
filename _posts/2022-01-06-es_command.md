---
title: es常用命令
tags: develop
author: Anyon
aside:
  toc: true
sidebar:
  nav: layouts
---



#### # 创建索引

```
下例为创建一个名为blogs的索引，分配给它3个主分片，每个主分片有1个副本分片

PUT /blogs
{
	"setting": {
		"number_of_shards": 3,
		"number_of_replicas": 1
	}
}

```



#### # 创建文档

```
PUT /{index}/{type}/{id}
{
	"field": "value",
	...
}

# 举例：其中website是index, blog是type， 123是指定id
PUT /website/blog/123
{
	"title": "My first blog entry",
	"text": "Just trying this out...",
	"date": "2014/01/01"
}

# 如果唯一标识没有指定的话，用下列方式创建文档
POST /website/blog/
{
	"title": "My first blog entry",
	"text": "Just trying this out...",
	"date": "2014/01/01"
}
```

#### 取回一个文档

```
# 加上pretty参数的作用是调用Elasticsearch中的pretty-print功能，使得json响应体更加可读
GET /website/blog/123?pretty

# 响应体为
{
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 1,
  "found" :    true,
  "_source" :  {
      "title": "My first blog entry",
      "text":  "Just trying this out...",
      "date":  "2014/01/01"
  }
}
```

#### 取回文档的一部分

```
# 取回文档中多个字段时，使用_source参数进行请求，多个字段用逗号分隔
GET /website/blog/123?_source=title,text

# 如果不要元数据，只返回_source字段内容，直接使用_source端点即可
GET /website/blog/123/_source
```

#### 命令行查询语句

```
# 查询pv_test中X6非空的信息
curl -XGET name:password@ip:port/pv_test/_search?pretty -H 'Content-Type:application/json' -d '{"query":{"bool":{"must":[{"exists":{"field":"X6"}}]}}, "_source":["X6"]}'
```

#### 命令行创建index

```
curl -XPUT name:password@ip:port/index -H 'Content-Type:application/json' -d '{"settings":{"number_of_replicas":1,"number_of_shards":1},"mappings":{"properties":{"TIME":{"type":"date"}}}}'

curl -XPUT name:password@ip:port/index -H 'Content-Type:application/json' -d '{"settings":{"number_of_replicas":1,"number_of_shards":1},"mappings":{"properties":{"TIME":{"type":"date"}}}}'
```

#### 命令行删除index

```
curl -XDELETE name:password@ip:port/index

curl -XDELETE name:password@ip:port/index
```

#### 命令行查询index

```
curl -XGET name:password@ip:port/index/_search?pretty

curl -XGET name:password@ip:port/index/_search?pretty
```

#### 查看指定索引大小

```
curl name:password@ip:port/_cat/indices/{索引名}

curl name:password@ip:port/_cat/indices/index
```





