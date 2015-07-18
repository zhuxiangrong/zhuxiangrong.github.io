---
layout: post
tags: 工具 hive
date: 2014-07-12 21:06
title: HIVE中随机选择样本
published: true
---

用HIVE做随机抽样的代码是下面这个样子的：

	select	id from mytable
	distribute by rand()
	order by rand()
	limit 300

distribute by rand() 是数据被随机的分道了不同的reducer上;order by rand()则在每个reducer上执行随机的排序。

HIVE提供了一些很有用的统计分析功能，慢慢研究与尝试！

[参考1](https://www.joefkelley.com/?p=736)	
[参考2](http://stackoverflow.com/questions/18951827/distributed-clause-in-hive)
