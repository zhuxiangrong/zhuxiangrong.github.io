---
layout: post
tags: 工具 hive
date: 2014-07-12 21:06
title: HIVE中随机选择样本
published: true
---

HIVE做随机抽样：

	select	id from mytable
	distribute by rand()
	order by rand()
	limit 300

distribute by rand() 是数据被随机的分道了不同的reducer上;order by rand()则在每个reducer上执行随机的排序。

随机分层抽样：

```
select
id,dt,rown
from
(select
id,dt,
row_number() OVER (PARTITION BY dt ORDER BY rand()) as rown
from
mytable
where
dt between 20150910 and 20150912
) a
where rown<10
```
这段代码是按照日期dt先将数据分组，然后每一组内随机排序，之后每一组内挑选出前10名。

HIVE提供了一些很有用的统计分析功能，慢慢研究与尝试！

[参考1](https://www.joefkelley.com/?p=736)	
[参考2](http://stackoverflow.com/questions/18951827/distributed-clause-in-hive)

