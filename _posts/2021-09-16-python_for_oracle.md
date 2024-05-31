---
title: python连接oracle
tags: python develop
author: Anyon
aside:
  toc: true
sidebar:
  nav: layouts
---



使用```sqlalchemy```连接Oracle数据库

```
from sqlalchemy import create_engine, Column, DATE, TIMESTAMP, NUMERIC
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base, DeclarativeMeta
import json
import datetime
import decimal

Base = declarative_base()

def to_dict(obj):
    if isinstance(obj.__class__, DeclarativeMeta):
        record = {}
        for field in [x for x in dir(obj) if
                      # 过滤属性
                      not x.startswith('_')
                      # 过滤方法属性
                      and hasattr(obj.__getattribute__(x), '__call__') == False
                      # 过滤不需要的属性
                      and x != 'metadata']:
            data = obj.__getattribute__(field)
            try:
                json.dumps(data,ensure_ascii=False)
                record[field] = data
            except TypeError:
                # 添加对datetime的处理
                if isinstance(data, datetime.datetime):
                    record[field] = data.isoformat(sep=" ")
                elif isinstance(data, datetime.date):
                    record[field] = data.isoformat()
                elif isinstance(data, datetime.timedelta):
                    record[field] = (datetime.datetime.min + data).time().isoformat()
                # 添加对decimal的处理
                elif isinstance(data, decimal.Decimal):
                    record[field] = float(data)
                else:
                    record[field] = None
        return record

class OPT_TG_RCD(Base):
	__tablename__ = 'OPT_TG_RCD'
	ID = Column(NUMERIC(0, 0), primary_key=True, info='台区用能优化记录标识')
	TG_ID = Column(NUMERIC(0, 0), info='台区标识')
	CRET_TIME = Column(TIMESTAMP, info='创建时间')
	MODI_TIME = Column(TIMESTAMP, info='修改时间')
	STRT_TIME = Column(DATE, info='优化日期')
	TYPE = Column(NUMERIC(0, 0), info='1日前优化或2日内优化')
	DURATION = Column(NUMERIC(10, 2), info='持续时长')
	OPT_REASON = Column(NUMERIC(0, 0), info='过载/重载/峰谷差过大/过电压/其他')
	BOOL_SENT = Column(NUMERIC(0, 0), info='是否下发指令（0：待下发 1：已下发 2：取消）')
	TARGET_TYPE = Column(NUMERIC(0, 0), info='优化目标模式')
	BOOL_SUCCESS = Column(NUMERIC(0, 0), info='是否成功')
	RESP_TYPE = Column(NUMERIC(0, 0), info='响应类型，削峰、填谷')


db_info = "oracle+cx_oracle://scott:nznz@ip:端口/orcl"
engine = create_engine(db_info)
DbSession = sessionmaker(bind=engine)

if __name__=="__main__":
	db = DbSession()
	try:
		# 查某个值
		res = db.query(OPT_TG_RCD.DURATION).filter_by(TG_ID=xxx).first()
		print(res)
	except Exception as e:
		print(str(e))
	finally:
		db.close()
	
	# 查多条记录
	db = DbSession()
	try:
		results = db.query(OPT_TG_RCD).filter_by(TG_ID=xxx).all()
		for ori_res in results:
			res = to_dict(ori_res)
			print(res)
	except Exception as e:
		print(str(e))
	finally:
		db.close()
```

环境准备：

> 查找数据库版本，下载对应的客户端，比如服务器上的版本时11g，我下载的是instantclient-basic-windows.x64-11.2.0.4.0.zip
>
> 解压后将解压路径添加到环境变量中
>
> 然后将所有的.dll文件拷到python环境的主目录下即可

